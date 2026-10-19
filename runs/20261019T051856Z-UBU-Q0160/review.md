# Model-Committee Review

Run: `20261019T051856Z-UBU-Q0160`  
Question: `UBU-Q0160`  
Base commit: `34ebafbf3a85d4f691205331a823ab0eefe838c9`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0160-mobile-repair-cache-pack-1`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 85.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0160-mobile-repair-cache-pack-1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0160-mobile-repair-cache-pack-1` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0160-mobile-representation-pack-d0387` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0160-mobile-representation-pack-d0387` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0160-mobile-repair-cache-pack-1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0160-mobile-repair-cache-pack-1` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0160-mobile-representation-pack-d0387` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0160-mobile-representation-pack-d0387` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0160-mobile-repair-cache-pack-1` | `openai` | `codex` | yes | 94 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch provides concrete cases for all five repair triggers and cache hits, misses and invalidation, while preserving protected-work rules, CPU certification, accelerator attribution and measurement gates. It makes useful, bounded progress without claiming an API selection or hardware result. | None | Fixture materialization, controller mappings and independently checked CPU goldens remain outstanding; the proposed cases do not establish executable parity or device qualification. |
| `UBU-Q0160-mobile-repair-cache-pack-1` | `openai` | `claude` | yes | 76 | The patch passes mechanical validation, touches only the three allowed files, leaves the UBU-Q0160 metadata line and Status: Open intact, and updates only the Resolution paragraph. It follows the established UBU-D0383 pattern by supplying a concrete repair/branch-cache fixture pack (five repair triggers, protected-skip confirmation, cache hit/miss, invalid-fact and stale-envelope cases) with internally consistent arithmetic (R1 shifts F to [300,900) within its [0,900) envelope; R4 correctly cannot fit 300 seconds in the 60 seconds before S). It is disciplined about not inventing hardware results, repair defaults, planning fields or an API selection, and explicitly keeps materialization, numerical, failure-ownership, resource and contention gates in force. Weaknesses are structural rather than defects: it is another specification layer on a question that can only be closed by on-device measurement, several cross-references (trigger names, D0383 parameters, decision ID, a new fact key) cannot be verified from the diff alone, and the block adds considerable density to §16.10.6. No decomposition was attempted. | Confirm the five repair trigger names and the Static/Flexible/Disposable/Protected class terms match DESIGN.md §16.5 and PLANNING_KERNEL_CONTRACT.md exactly before merge., Confirm UBU-D0387 is the next unused decision ID and that T, seed 160, the 3600-second window and 60-second delta are defined in the UBU-D0383 block it reuses. | The question is blocked on physically connected device measurements; this is the sixth consecutive specification-only decision on UBU-Q0160 (after D0358, D0364, D0370, D0377, D0383) and adds no measurement, harness code or fixture files, so marginal value toward actually answering the question is diminishing., The new block depends on definitions 'above' from the UBU-D0383 structural pack (T, the 3600-second window, 60-second planning delta, seed 160, Static S anchor). The patch applied cleanly so the context lines exist, but these parameter references could not be verified against the base commit from this scoring context., The five trigger identifiers (late_task, skipped_task, fatigue_report, approaching_static_task, missing_prerequisite) and vocabulary such as stale_after, Snapshot, continuation refs and chunk boundary are asserted to match DESIGN.md §16.5 and PLANNING_KERNEL_CONTRACT.md; a mismatch with the canonical enum names would make the fixture pack misname triggers., The fixture introduces a synthetic fact path facts.project.mobile_fixture.ready. If the facts namespace is a controlled vocabulary, this is a de facto new key rather than pure test data; the text is careful not to add PlanningRequest fields but does not address fact-key governance., UBU-D0387 is assumed to be the next free decision ID; the diff context does not show the preceding decision's ID, so a collision cannot be excluded from the patch alone., The DESIGN.md addition is a single dense block with a seven-row table inside an already long §16.10.6; readability of the section continues to degrade, and the decision is marked Accepted without an operator having seen it, though this matches the prior fixture-pack precedent and the decision selects no API. |
| `ubu-q0160-mobile-representation-pack-d0387` | `anthropic` | `codex` | yes | 80 | Mechanical validation confirms clean application, allowed files and preserved question schema. The proposal usefully concretizes representation fixtures while retaining CPU certification, fallback accounting, hardware measurement requirements and the Open API decision. P3 needs correction before its mandatory acceptance rules are reproducible across candidate representations. | Define P3 boundaries using contract-reachable values and explicit unit conversion. Specify the greatest supported value and first unsupported value when the declared bound is not aligned to the planning delta., Provide separate constructions for offset, duration and intermediate arithmetic limits, identifying the quantity under test and keeping other quantities within supported ranges. | P3 requires chains with durations divisible by 60 to end exactly at arbitrary declared bounds. Bounds such as 2147483647 seconds cannot satisfy that construction., Testing a chain's final end offset does not necessarily exercise individual duration limits or other intermediate arithmetic bounds. |
| `ubu-q0160-mobile-representation-pack-d0387` | `anthropic` | `claude` | yes | 82 | The patch passed mechanical validation and the allowlist, touches only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md, and keeps UBU-Q0160 at Status: Open with its metadata line unchanged, which is correct for a human-approval architecture question blocked on physically connected device measurements. It is directly responsive to the stated gap: UBU-D0383's text and the question's resolution both say UBU-D0370's representation and range cases remain required, and this pack materializes them with concrete, independently checkable values. The arithmetic checks out: 2026-10-16T09:00:00Z is Unix 1792141200 (above 2^30, float32 spacing 128 s), 2038-01-19T03:00:00Z is 2147483648 minus 848, 2106-02-07T06:10:00Z is 4294967296 minus 1096, and 1970-01-02T00:00:00Z is 86400, so the windows do straddle the signed and unsigned 32-bit wrap points and sit where single-precision seconds cannot resolve a 60-second separation. Expected results reuse already-frozen M2, M5 and M6 semantics, no representation width, tolerance, API or schema field is introduced, and the above-bound case correctly routes to CPU fallback under D0364 accounting rather than a semantic rejection. The near-origin control is a sound design choice for separating magnitude failures from rule failures. Deductions are for the likely out-of-order placement in DECISIONS.md, the unverifiable dependence on D0383's exact placement offsets, the inclusive versus exclusive bound ambiguity in P3, and the general concern that another specification-only decision adds limited movement toward the actual API selection. | At merge, verify the position of the UBU-D0387 block in DECISIONS.md and move it after UBU-D0386 if the file keeps decisions in numeric order., Confirm against the frozen UBU-D0383 M2 placements that each B interval covers offset 848 and offset 1096, or adjust W1/W2 so the wrap seconds fall inside every compared placement., Clarify in P3 that 'bound' means the largest supported value inclusive, or restate the three cases as below, at and above the last supported value in the mapping's declared unit. | DECISIONS.md ordering: the new UBU-D0387 block is inserted immediately after UBU-D0383 using that block as context. The author states UBU-D0384 to UBU-D0386 exist but were not visible, so if any of them follow D0383 in the file the patch leaves DECISIONS.md out of numeric order., P1's claim that every M2 B placement at W1 contains second 2^31 (offset 848) and at W2 contains 2^32 (offset 1096) depends on the exact offsets frozen in UBU-D0383's structural pack, which are not visible in this request; the author asserts offsets 540 to 1260 but this could not be independently confirmed., P3 says the request 'exactly at the bound' must execute with exact parity, which presumes each declared bound is inclusive. If a candidate's declared bound is exclusive (for example a 2^31 limit whose largest representable value is 2^31 minus one), the 'at bound' case becomes an above-bound case and the expected disposition flips., P2 hardcodes a 256-slot Task mask for the slot-boundary case while the batch case is parametric on the mapping's declared maximum. If the frozen mappings declare a different slot count, the slot case must be re-derived., This is the sixth specification-only decision narrowing UBU-Q0160 without a fixture file or device measurement; the question remains blocked on hardware and the marginal value of further protocol text is declining, though this pack does fill a gap D0383 explicitly left open. |

## Selected Summary

Specify concrete repair and branch-cache fixtures for the mobile API comparison; retain measured API selection as Open.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- DECISIONS.md ordering: the new UBU-D0387 block is inserted immediately after UBU-D0383 using that block as context. The author states UBU-D0384 to UBU-D0386 exist but were not visible, so if any of them follow D0383 in the file the patch leaves DECISIONS.md out of numeric order.
- Fixture materialization, controller mappings and independently checked CPU goldens remain outstanding; the proposed cases do not establish executable parity or device qualification.
- P1's claim that every M2 B placement at W1 contains second 2^31 (offset 848) and at W2 contains 2^32 (offset 1096) depends on the exact offsets frozen in UBU-D0383's structural pack, which are not visible in this request; the author asserts offsets 540 to 1260 but this could not be independently confirmed.
- P2 hardcodes a 256-slot Task mask for the slot-boundary case while the batch case is parametric on the mapping's declared maximum. If the frozen mappings declare a different slot count, the slot case must be re-derived.
- P3 requires chains with durations divisible by 60 to end exactly at arbitrary declared bounds. Bounds such as 2147483647 seconds cannot satisfy that construction.
- P3 says the request 'exactly at the bound' must execute with exact parity, which presumes each declared bound is inclusive. If a candidate's declared bound is exclusive (for example a 2^31 limit whose largest representable value is 2^31 minus one), the 'at bound' case becomes an above-bound case and the expected disposition flips.
- Testing a chain's final end offset does not necessarily exercise individual duration limits or other intermediate arithmetic bounds.
- The DESIGN.md addition is a single dense block with a seven-row table inside an already long §16.10.6; readability of the section continues to degrade, and the decision is marked Accepted without an operator having seen it, though this matches the prior fixture-pack precedent and the decision selects no API.
- The five trigger identifiers (late_task, skipped_task, fatigue_report, approaching_static_task, missing_prerequisite) and vocabulary such as stale_after, Snapshot, continuation refs and chunk boundary are asserted to match DESIGN.md §16.5 and PLANNING_KERNEL_CONTRACT.md; a mismatch with the canonical enum names would make the fixture pack misname triggers.
- The fixture introduces a synthetic fact path facts.project.mobile_fixture.ready. If the facts namespace is a controlled vocabulary, this is a de facto new key rather than pure test data; the text is careful not to add PlanningRequest fields but does not address fact-key governance.
- The new block depends on definitions 'above' from the UBU-D0383 structural pack (T, the 3600-second window, 60-second planning delta, seed 160, Static S anchor). The patch applied cleanly so the context lines exist, but these parameter references could not be verified against the base commit from this scoring context.
- The question is blocked on physically connected device measurements; this is the sixth consecutive specification-only decision on UBU-Q0160 (after D0358, D0364, D0370, D0377, D0383) and adds no measurement, harness code or fixture files, so marginal value toward actually answering the question is diminishing.
- This is the sixth specification-only decision narrowing UBU-Q0160 without a fixture file or device measurement; the question remains blocked on hardware and the marginal value of further protocol text is declining, though this pack does fill a gap D0383 explicitly left open.
- UBU-D0387 is assumed to be the next free decision ID; the diff context does not show the preceding decision's ID, so a collision cannot be excluded from the patch alone.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T051856Z-UBU-Q0160/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T051856Z-UBU-Q0160/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T051856Z-UBU-Q0160"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
