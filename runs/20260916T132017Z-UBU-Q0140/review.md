# Model-Committee Review

Run: `20260916T132017Z-UBU-Q0140`  
Question: `UBU-Q0140`  
Base commit: `3bfc8e22fc26bccbb616a65622e6853dfe39099e`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0140-hybrid-causality-v1`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0140-hybrid-causality-v1` |
| `work-score` | `claude` | `sonnet` | `ubu-q0140-hybrid-causality-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0140-hybrid-causality-v1` |
| `work-score` | `claude` | `sonnet` | `ubu-q0140-hybrid-causality-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0140-hybrid-causality-v1` | `openai` | `codex` | yes | 93 | The proposal directly resolves UBU-Q0140 with a clear hybrid mechanism that combines content-addressed SyncStatements, per-origin HLC ticks, causal_parents as the authoritative DAG, and observed object and policy versions as conflict preconditions. It preserves the open-question schema, updates resolution metadata, adds a concise auditable decision, modifies only the allowed files, and has a clean mechanical validation result. The tradeoff analysis rejects the listed alternatives with appropriate reasoning. Remaining risks are mostly future specification detail rather than blockers for this decision patch. | None | The decision is still fairly high-level and may need later schema-level precision for exact field formats, HLC tuple representation, parent cardinality, and canonicalization rules., Adding an accepted decision without modifying the referenced contract sections leaves implementation detail centralized in DECISIONS.md rather than fully propagated, though that is consistent with the allowed file scope. |
| `ubu-q0140-hybrid-causality-v1` | `openai` | `claude` | yes | 88 | The patch applies cleanly and touches only OPEN_QUESTIONS.md and DECISIONS.md, both allowed files per mechanical validation. It preserves the OPEN_QUESTIONS.md single-line metadata schema (Status/Priority/.../Scored from commit) and correctly threads Resolved by: UBU-D0247 back into the question entry. The new DECISIONS.md entry (UBU-D0247) directly answers the question as posed — it selects a specific hybrid mechanism (content-addressed signed SyncStatements + per-origin HLC + causal_parents DAG + observed_versions preconditions) rather than punting, and explicitly rejects the other named alternatives (pure Lamport, pure vector clocks, pure per-object counters, pure content-addressed bundles) with concrete reasoning tied to the multi-device/partial-replica sync model. Scope is tight: no unrelated files touched, no extraneous new questions introduced. requires_human_review is correctly set true given this is a Data model decision type needing human approval. Minor deduction for introducing new field names (observed_policy_versions, admitted_time) whose consistency with the referenced contract sections isn't verified in this patch alone. | None | This is a foundational data-model decision affecting Phase 2 sync; if the hybrid HLC+DAG+content-address design has a flaw it will be costly to unwind since it blocks UBU-Q0130 and Phase 2 sync statement compatibility., The decision text introduces new terminology (observed_policy_versions, admitted_time) that must stay consistent with DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8/§15, which the patch does not modify — a human reviewer should confirm no drift with the referenced contract sections., Tie-breaking order (HLC tick, origin Device ID, statement ID) is asserted but not justified in depth; worth confirming determinism holds for concurrent partial-replica scenarios. |

## Selected Summary

Resolve UBU-Q0140 by selecting a hybrid Phase 2 causality mechanism: content-addressed signed SyncStatements as the durable append-only record, per-origin hybrid logical clocks for compact tie breaking and progress, causal_parents as the authoritative statement DAG, and observed object/policy versions as deterministic conflict preconditions.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DECISIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Adding an accepted decision without modifying the referenced contract sections leaves implementation detail centralized in DECISIONS.md rather than fully propagated, though that is consistent with the allowed file scope.
- The decision is still fairly high-level and may need later schema-level precision for exact field formats, HLC tuple representation, parent cardinality, and canonicalization rules.
- The decision text introduces new terminology (observed_policy_versions, admitted_time) that must stay consistent with DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8/§15, which the patch does not modify — a human reviewer should confirm no drift with the referenced contract sections.
- This is a foundational data-model decision affecting Phase 2 sync; if the hybrid HLC+DAG+content-address design has a flaw it will be costly to unwind since it blocks UBU-Q0130 and Phase 2 sync statement compatibility.
- Tie-breaking order (HLC tick, origin Device ID, statement ID) is asserted but not justified in depth; worth confirming determinism holds for concurrent partial-replica scenarios.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260916T132017Z-UBU-Q0140/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260916T132017Z-UBU-Q0140/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260916T132017Z-UBU-Q0140"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
