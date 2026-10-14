# Model-Committee Review

Run: `20261014T110601Z-UBU-Q0160`  
Question: `UBU-Q0160`  
Base commit: `d91f3aeef2b51a1cfb1d279736fcbcbf69bf9a05`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 83.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering` | `openai` | `codex` | yes | 93 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch usefully specifies representation bounds, arithmetic-boundary fixtures and explicit CPU routing while preserving exact parity, existing budgets and fallback accounting. It advances qualification requirements; API selection appropriately remains Open pending measurements and operator review. | None | The empirical blocker remains: no harness implementation or physical-device measurements establish API suitability. |
| `ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering` | `openai` | `claude` | yes | 74 | Patch applies, passes allowlist and schema checks, and confines OPEN_QUESTIONS.md edits to UBU-Q0160 while keeping Status: Open and Resolved by: None, which is correct given the question requires physical device measurements and human approval. The content addresses a genuine gap: the existing §16.10.6 matrix records supported ops/dtypes but does not test whether coordinate representation (e.g., fp32 timestamps at epoch scale), equality boundaries, or duration accumulation preserve hard-constraint decisions. The proposed fixtures (large timestamps with unit separations, translated origins, equality endpoints, index/padding bounds, near-limit sums) are concrete, reproducible, and explicitly decline to select an API or dtype. It coheres with UBU-D0283 parity and UBU-D0364 accounting by deferring to them. Deductions: incremental rather than resolving, risk of a fabricated field identifier, unspecified interaction with D0364 outcome categories, and dense prose. Overall a valid, auditable, directly responsive narrowing of an Open question that cannot yet be closed. | Confirm `planning_delta_seconds` is a defined field in PLANNING_KERNEL_CONTRACT.md; otherwise replace it with the actual field name or a generic phrase such as 'the planning delta'., Add one sentence specifying how range-triggered CPU routing is recorded in the UBU-D0364 trial manifest (which outcome category it belongs to) so fallback accounting stays comparable across candidates. | Third consecutive protocol-only decision on UBU-Q0160 (after UBU-D0358 and UBU-D0364) with no hardware measurement; requirement accretion raises harness cost before any trial exists and may delay the actual gate., References `planning_delta_seconds` as if it were an established kernel-contract field; if PLANNING_KERNEL_CONTRACT.md does not define that identifier, the design text names a nonexistent schema element., Introduces a new fallback class (otherwise-valid request outside accelerator bounds routed to CPU) without stating how UBU-D0364's accounting classifies it: hybrid mapping, full CPU fallback, or a separate column. Ambiguity could let range-based fallback be reported inconsistently across candidates., The two inserted DESIGN.md paragraphs are dense and long relative to the surrounding §16.10.6 text, making the exact set of new obligations harder to audit and to turn into fixtures., The requirement that lowering preserve exact interval/dependency/mask semantics may effectively force all structural operations CPU-resident on fp32-only accelerators, which is correct for parity but should be acknowledged as a likely outcome rather than left implicit. |

## Selected Summary

Require lossless structural lowering and representation-boundary fixtures in the mobile API comparison; retain CPU routing and keep API selection open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Introduces a new fallback class (otherwise-valid request outside accelerator bounds routed to CPU) without stating how UBU-D0364's accounting classifies it: hybrid mapping, full CPU fallback, or a separate column. Ambiguity could let range-based fallback be reported inconsistently across candidates.
- References `planning_delta_seconds` as if it were an established kernel-contract field; if PLANNING_KERNEL_CONTRACT.md does not define that identifier, the design text names a nonexistent schema element.
- The empirical blocker remains: no harness implementation or physical-device measurements establish API suitability.
- The requirement that lowering preserve exact interval/dependency/mask semantics may effectively force all structural operations CPU-resident on fp32-only accelerators, which is correct for parity but should be acknowledged as a likely outcome rather than left implicit.
- The two inserted DESIGN.md paragraphs are dense and long relative to the surrounding §16.10.6 text, making the exact set of new obligations harder to audit and to turn into fixtures.
- Third consecutive protocol-only decision on UBU-Q0160 (after UBU-D0358 and UBU-D0364) with no hardware measurement; requirement accretion raises harness cost before any trial exists and may delay the actual gate.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T110601Z-UBU-Q0160/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T110601Z-UBU-Q0160/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T110601Z-UBU-Q0160"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
