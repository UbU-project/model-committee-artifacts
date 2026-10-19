# Model-Committee Review

Run: `20261019T045201Z-UBU-Q0166`  
Question: `UBU-Q0166`  
Base commit: `a4221326d00a2a3b7ff3e80998509b82262b4d75`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0166-openai-discovery-host-inventory`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 81.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-openai-discovery-host-inventory` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-openai-discovery-host-inventory` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0166-d0386-fidelity-specimen-stage-dispositions` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0166-d0386-fidelity-specimen-stage-dispositions` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-openai-discovery-host-inventory` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-openai-discovery-host-inventory` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0166-d0386-fidelity-specimen-stage-dispositions` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0166-d0386-fidelity-specimen-stage-dispositions` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0166-openai-discovery-host-inventory` | `openai` | `codex` | yes | 88 | Mechanical validation passes. Independent inspection confirms the five revision pins, clean tracked files, 630-file search and reported carrier representations. The patch implements a useful bounded follow-up to UBU-D0385, changes only allowed Markdown files, and preserves the question metadata, Open status and closure criteria. It correctly distinguishes missing implementation evidence from a governing schema gap. The score reflects prerequisite progress rather than a completed host mapping or preservation result. | None | The negative search result is bounded to the inspected revisions and tracked directories; generic or alternate mappings remain possible., Complete evidence resolution, qualifier fidelity and policy-authorized path fixtures remain unexecuted. |
| `UBU-Q0166-openai-discovery-host-inventory` | `openai` | `claude` | yes | 74 | Mechanical validation passes (applies cleanly, allowlist OK, question schema OK). The patch is docs-only across DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md, keeps UBU-Q0166 Open with its single-line metadata intact, adds UBU-D0386 in sequence, and does not alter closure criteria, schema, pins or migrations. It is directly responsive to UBU-D0385's stated next step (investigate a complete Discovery evidence revision and resolved candidate/field links): it attempted to locate that host path, found no dedicated implementation or resolver, and records the result as an explicitly unexecuted path rather than inferring a governing schema gap. The claim discipline is careful and matches the question's established evidentiary standards (carrier retention vs. resolved fidelity, no invented candidate kind, absence not equated with gap). The score is held in the 70s because the contribution is a bounded negative source inventory with no new runtime evidence, and because its correctness depends on host-repo inspection claims that cannot be independently audited from the patch alone. Optional improvements: restate the five host revision hashes in the DESIGN.md table, and record the tracked-file count and exact grep invocation so the negative result is reproducible. | None | The inventory's value rests entirely on unverifiable host-source claims (specific file paths, function names such as object_type_from_str and store_worker_submission, and a negative case-insensitive grep across five repos). A single wrong path or an overly narrow search scope would make the 'no Discovery implementation located' finding misleading, and nothing in the patch lets a reviewer re-derive it without re-running the inspection., Source-inspection only: it executes no validator, round-trip, service, review or exchange fixture, so it advances UBU-Q0166 only marginally. It effectively defers the next fidelity run behind an unidentified or not-yet-implemented evidence write/read path, which could stall the question if no one picks up the separately reviewed implementation work., The five host revisions are referenced indirectly via UBU-D0323 rather than restated in the new DESIGN.md table, so the pin is one hop removed from the evidence it bounds., The DESIGN.md insertion lands in the §21.1.1 evidence-index region directly before the 'Supported operations' contract paragraph; it reads as an appended probe record, which is consistent with prior entries but makes the section progressively harder to navigate. |
| `ubu-q0166-d0386-fidelity-specimen-stage-dispositions` | `anthropic` | `codex` | yes | 67 | Mechanical validation passes, allowed files are respected, and Q0166 correctly remains Open without a schema-gap claim. The specimen and reporting vocabulary are useful preparation, but the actual mapping investigation remains unimplemented and the proposed reporting gate needs correction. | Execute and document at least the next bounded Discovery mapping probe with pinned host fields, resolved revisions and independent readback comparisons; keep subsequent unexecuted paths explicit., Define rejection obligations per stage, permit appropriate evidence retention before the executable boundary, and specify downstream dispositions after required rejection., Define the confidence-number domain separately from the integer domain and state the independent numeric comparison oracle. | The patch specifies a useful fixture but supplies no revision-pinned host mapping, resolved stored Discovery records or executed round-trip evidence., The negative-control rule treats native ok, storage and API reads as validation failures without distinguishing carrier acceptance from the required adapter rejection before executable review or admission., The closure rule leaves unclear how validation-only stages and downstream stages intentionally unexecuted after required rejection can satisfy the closed disposition scheme., Confidence values 0.82 and 0.4 contradict the statement that every number belongs to the safe-integer domain. |
| `ubu-q0166-d0386-fidelity-specimen-stage-dispositions` | `anthropic` | `claude` | yes | 70 | The patch applies and passes the allowlist and question-schema checks; UBU-Q0166 stays Status: Open with Resolved by: None and only the three documentation files change. It is directly responsive to the Current direction, which ends with UBU-D0385's call to investigate a complete Discovery evidence revision with resolved candidate and field links. Every prior probe used a carrier specimen with no claim content, so freezing an exact, auditable specimen is a genuine precondition for that investigation rather than filler. The specimen F1 is internally consistent: distinct observation and recording times, per-claim confidence with scale, method and source, contradiction and shares_source relations, verification carried as separate evidence with an inconclusive result and null confidence, and one field citing conflicting claims. The three negative controls each differ by exactly one change. The closed disposition vocabulary addresses the ambiguity UBU-D0385 noted between validation-only acceptance and exact fidelity, and it requires decoder-independent oracles in line with UBU-D0379. The decision is careful to claim no runtime result, no mapping, no repair and no schema or pin change. The score is held to the low seventies because the work is preparatory: it produces no round-trip evidence, the question asks for exactly that evidence, and a few wording issues (fixture-local sub-keys unflagged in DESIGN.md, unnamed positive scenarios, a class assigned before the rule permits it) leave the next prober some interpretation to do. | State in the DESIGN.md specimen text that the sub-object keys for content, attribution, confidence and basis are fixture-local and not a perceptual-qualifiers/1 profile schema decision, or cite the UBU-D0321 rows that fix them., Name the positive scenarios that count toward 'every required stage' or cross-reference the exact rows of the existing required-fixtures table., Either drop the `missing_adapter_validation` label on UBU-D0334's unknown-profile case or mark it provisional until the adapter path is pinned, so the text matches its own rule for when a class is assigned., Reword the Resolution entry so the disposition-based rule is explicitly a restatement of the existing UBU-D0321 gate, not a new closure criterion. | Specification only: the proposal executes no probe, so it adds an eighth non-closing decision to UBU-Q0166 and leaves the Discovery-evidence host-mapping investigation named by UBU-D0385 still unrun., The fixture fixes sub-object keys inside content, attribution, confidence and basis that the validation notes admit UBU-D0321 did not pin. The DESIGN.md text does not carry that caveat, so the specimen can read as implicitly defining profile sub-schema while the decision states no schema change occurs., The closure rule refers to 'each positive scenario above' without naming the scenarios, so which existing section 21.1.1 scenarios count as required stages is left to the reader., The vocabulary says a `changed` class is assigned once the adapter path is pinned, yet the decision already labels UBU-D0334's unknown-profile case a `missing_adapter_validation` finding before any adapter path is pinned. This is a small internal tension and an interpretive claim about a prior result made without new execution., The OPEN_QUESTIONS.md Resolution phrase 'Closure now requires' can be read as replacing UBU-D0321's gate rather than restating it in the new vocabulary, even though the decision text frames it as a restatement., The base commit could not be inspected from this session; the apply result and the claim that prior probes used only content-free carrier specimens rest on the mechanical validation and the proposal's own account., The authoring model name matches the scoring model, although the harness labels the providers as `anthropic` and `claude`; treat this score as a cross-score only under that labeling. |

## Selected Summary

Record a pinned Discovery host-path inventory and the unresolved evidence-revision mapping prerequisite.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Complete evidence resolution, qualifier fidelity and policy-authorized path fixtures remain unexecuted.
- Confidence values 0.82 and 0.4 contradict the statement that every number belongs to the safe-integer domain.
- Source-inspection only: it executes no validator, round-trip, service, review or exchange fixture, so it advances UBU-Q0166 only marginally. It effectively defers the next fidelity run behind an unidentified or not-yet-implemented evidence write/read path, which could stall the question if no one picks up the separately reviewed implementation work.
- Specification only: the proposal executes no probe, so it adds an eighth non-closing decision to UBU-Q0166 and leaves the Discovery-evidence host-mapping investigation named by UBU-D0385 still unrun.
- The DESIGN.md insertion lands in the §21.1.1 evidence-index region directly before the 'Supported operations' contract paragraph; it reads as an appended probe record, which is consistent with prior entries but makes the section progressively harder to navigate.
- The OPEN_QUESTIONS.md Resolution phrase 'Closure now requires' can be read as replacing UBU-D0321's gate rather than restating it in the new vocabulary, even though the decision text frames it as a restatement.
- The authoring model name matches the scoring model, although the harness labels the providers as `anthropic` and `claude`; treat this score as a cross-score only under that labeling.
- The base commit could not be inspected from this session; the apply result and the claim that prior probes used only content-free carrier specimens rest on the mechanical validation and the proposal's own account.
- The closure rule leaves unclear how validation-only stages and downstream stages intentionally unexecuted after required rejection can satisfy the closed disposition scheme.
- The closure rule refers to 'each positive scenario above' without naming the scenarios, so which existing section 21.1.1 scenarios count as required stages is left to the reader.
- The five host revisions are referenced indirectly via UBU-D0323 rather than restated in the new DESIGN.md table, so the pin is one hop removed from the evidence it bounds.
- The fixture fixes sub-object keys inside content, attribution, confidence and basis that the validation notes admit UBU-D0321 did not pin. The DESIGN.md text does not carry that caveat, so the specimen can read as implicitly defining profile sub-schema while the decision states no schema change occurs.
- The inventory's value rests entirely on unverifiable host-source claims (specific file paths, function names such as object_type_from_str and store_worker_submission, and a negative case-insensitive grep across five repos). A single wrong path or an overly narrow search scope would make the 'no Discovery implementation located' finding misleading, and nothing in the patch lets a reviewer re-derive it without re-running the inspection.
- The negative search result is bounded to the inspected revisions and tracked directories; generic or alternate mappings remain possible.
- The negative-control rule treats native ok, storage and API reads as validation failures without distinguishing carrier acceptance from the required adapter rejection before executable review or admission.
- The patch specifies a useful fixture but supplies no revision-pinned host mapping, resolved stored Discovery records or executed round-trip evidence.
- The vocabulary says a `changed` class is assigned once the adapter path is pinned, yet the decision already labels UBU-D0334's unknown-profile case a `missing_adapter_validation` finding before any adapter path is pinned. This is a small internal tension and an interpretive claim about a prior result made without new execution.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T045201Z-UBU-Q0166/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T045201Z-UBU-Q0166/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T045201Z-UBU-Q0166"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
