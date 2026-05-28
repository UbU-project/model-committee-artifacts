# Model-Committee Review

Run: `20260528T165754Z-UBU-Q0071`  
Question: `UBU-Q0071`  
Base commit: `b2a846b0205508fd94fe9750b42ff1d4e381c57a`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0071-legitimization-cost-model`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0071-legitimization-cost-model` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0071-legitimization-cost-model` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0071-legitimization-cost-model` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0071-legitimization-cost-model` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0071-legitimization-cost-model` | `openai-codex` | `codex` | yes | 90 | The proposal directly resolves all five subquestions with a minimal decision, design update, solved-question tombstone, and contract extension. It preserves the baseline feasible Plan concept, keeps hard legitimacy as the validity gate, treats graded legitimacy as advisory, identifies a sufficient MVP semi-legitimization heuristic set, and protects recuperative work from being treated as optional gap filling. Scope is limited to canonical planning documents and the mechanical validation passes. | None | The cost model is qualitative rather than measured; it defines when full legitimization runs but does not add concrete complexity bounds or instrumentation requirements., PLANNING_KERNEL_CONTRACT.md only adds the semi-legitimization summary fields, while some newly named full-legitimization outputs such as protected support refs and legitimacy margins may need later schema tightening. |
| `codex-ubu-q0071-legitimization-cost-model` | `openai-codex` | `claude` | yes | 88 | The patch cleanly resolves all five subquestions of UBU-Q0071. DESIGN.md §15.2.2 now defines the cost boundary (finalist oracle vs. cheap pruning), names the six MVP semi-legitimization heuristics, establishes legitimacy as both binary (validity gate) and graded (advisory ranking), specifies when a brittle Plan may displace a humane Plan, and re-classifies recuperative support as protected ordinary Tasks rather than gap-filling. DECISIONS.md adds UBU-D0211 with full consequence enumeration. OPEN_QUESTIONS.md converts UBU-Q0071 to a compact solved tombstone in the expected format. PLANNING_KERNEL_CONTRACT.md adds a concrete semi_legitimization_summary schema. All changes are scoped to the four relevant canonical files and no new open questions are introduced. Minor risks around enum definition duplication and underspecified numeric field semantics do not require fixes before selection. | None | legitimacy_delta_estimate in PLANNING_KERNEL_CONTRACT.md is typed as 'optional numeric' with no explicit units or normalization reference, which may create ambiguity for implementors comparing across Plans with different value scales, The semi-legitimization result enum (passes_cheap_checks, reject_obvious, needs_full_legitimization) is defined inline in two places (DESIGN.md and PLANNING_KERNEL_CONTRACT.md) without a shared type definition, creating a minor divergence risk if one is updated independently, The condition under which a brittle Plan may beat a humane Plan lists five pass criteria but does not specify whether all five must be satisfied simultaneously or whether they form a priority-ordered checklist, leaving room for inconsistent implementation interpretations |

## Selected Summary

Resolve UBU-Q0071 by defining full legitimization as a CPU-certified finalist oracle, semi-legitimization as cheap conservative pruning, and recuperative support as protected Plan support work rather than optional gap filling.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`
- `PLANNING_KERNEL_CONTRACT.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- PLANNING_KERNEL_CONTRACT.md only adds the semi-legitimization summary fields, while some newly named full-legitimization outputs such as protected support refs and legitimacy margins may need later schema tightening.
- The condition under which a brittle Plan may beat a humane Plan lists five pass criteria but does not specify whether all five must be satisfied simultaneously or whether they form a priority-ordered checklist, leaving room for inconsistent implementation interpretations
- The cost model is qualitative rather than measured; it defines when full legitimization runs but does not add concrete complexity bounds or instrumentation requirements.
- The semi-legitimization result enum (passes_cheap_checks, reject_obvious, needs_full_legitimization) is defined inline in two places (DESIGN.md and PLANNING_KERNEL_CONTRACT.md) without a shared type definition, creating a minor divergence risk if one is updated independently
- legitimacy_delta_estimate in PLANNING_KERNEL_CONTRACT.md is typed as 'optional numeric' with no explicit units or normalization reference, which may create ambiguity for implementors comparing across Plans with different value scales

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T165754Z-UBU-Q0071/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T165754Z-UBU-Q0071/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T165754Z-UBU-Q0071"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
