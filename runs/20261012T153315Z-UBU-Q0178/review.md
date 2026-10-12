# Model-Committee Review

Run: `20261012T153315Z-UBU-Q0178`  
Question: `UBU-Q0178`  
Base commit: `bb67912243b1b9f5e1a0f2414488681d2bdb1dee`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0178-openai-local-compute-cost-envelopes`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 86.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0178-openai-local-compute-cost-envelopes` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0178-openai-local-compute-cost-envelopes` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0178-openai-local-compute-cost-envelopes` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0178-openai-local-compute-cost-envelopes` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0178-openai-local-compute-cost-envelopes` | `openai` | `codex` | yes | 92 | Mechanical validation passes, changes stay within allowed files, and Q0178's Open metadata is preserved. The proposal adds concrete bounded local routes, matched-workload cost and effort comparisons, and useful end-to-end acceptance criteria while preserving existing security and retention gates. It appropriately leaves hardware, latency, mobile API and release decisions in their existing questions. This is substantial design progress, with practical affordability still awaiting evidence. | None | No measured configuration or cost envelope is established; affordable multimodal operation remains unproven., Both routes assume suitable owned hardware, limiting conclusions about users without access to that hardware. |
| `UBU-Q0178-openai-local-compute-cost-envelopes` | `openai` | `claude` | yes | 80 | Patch applies cleanly, touches only the three allowlisted files, and passed question-schema validation. Q0178's metadata line is untouched, Status stays Open, and the Resolution is expanded in the same 'Open. UBU-D033x answers X only. Close after ...' pattern already used for Q0177, giving consistent, auditable closure criteria. The content is directly responsive: it covers every element of the current direction (smaller/quantized local models, bounded snapshots, CPU inference, qualified latency classes, operator-owned trusted worker hardware, no hosted dependency) and adds a concrete cost-evidence contract (predeclared budgets, dated regional prices, energy/storage/effort accounting, incremental vs. replacement costs, 'lower-cost' only against a named comparator) that operationalizes 'measure tradeoffs before claiming reach or costs'. It explicitly refuses to let a text-only fallback or smaller model stand in for affordable multimodal reach, which guards against a cheap false close. It correctly defers to Q0177/Q0168/Q0160/Q0183 for adjacent scope. Deductions are for verbosity and duplication of D0331 gates in DESIGN.md, the lack of a non-exhaustive clause on candidate routes, and the fact that this is a narrowing rather than a selection. No decomposition occurred and none was needed, since Q0178 itself remains the vehicle for the eventual evidence-backed selection. | None | Protocol-only narrowing: UBU-D0332 names two candidate routes and a cost-evidence contract but selects no path, so the question's core product decision remains unanswered until real-device trials are run and reviewed., The 'existing personal worker' route presumes the user already owns a laptop/desktop; for the economically precarious population the question targets, this route may be unavailable, and the text only partly mitigates by separating incremental outlay from acquisition cost., The DESIGN.md §1 insertion is seven dense paragraphs that largely restate UBU-D0331 gates (consent, freshness, retention, review, Plan certification); this duplication increases maintenance burden and the chance of drift between the two sections., The decision reads as limiting comparison to exactly two routes; it does not state that the candidate list is non-exhaustive, which could be interpreted as excluding other lower-cost paths (shared/community hardware, refurbished devices) from future evaluation., Blanket exclusion of any hosted inference, paid API credential or subscription for these routes is stricter than the current direction's 'hidden hosted-model dependency' wording; this is defensible for 'local compute paths' but slightly tightens the question's framing. |

## Selected Summary

Define bounded CPU inference and existing personal-worker candidates with measured cost qualification; keep UBU-Q0178 open pending evidence-backed selection.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Blanket exclusion of any hosted inference, paid API credential or subscription for these routes is stricter than the current direction's 'hidden hosted-model dependency' wording; this is defensible for 'local compute paths' but slightly tightens the question's framing.
- Both routes assume suitable owned hardware, limiting conclusions about users without access to that hardware.
- No measured configuration or cost envelope is established; affordable multimodal operation remains unproven.
- Protocol-only narrowing: UBU-D0332 names two candidate routes and a cost-evidence contract but selects no path, so the question's core product decision remains unanswered until real-device trials are run and reviewed.
- The 'existing personal worker' route presumes the user already owns a laptop/desktop; for the economically precarious population the question targets, this route may be unavailable, and the text only partly mitigates by separating incremental outlay from acquisition cost.
- The DESIGN.md §1 insertion is seven dense paragraphs that largely restate UBU-D0331 gates (consent, freshness, retention, review, Plan certification); this duplication increases maintenance burden and the chance of drift between the two sections.
- The decision reads as limiting comparison to exactly two routes; it does not state that the candidate list is non-exhaustive, which could be interpreted as excluding other lower-cost paths (shared/community hardware, refurbished devices) from future evaluation.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T153315Z-UBU-Q0178/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T153315Z-UBU-Q0178/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T153315Z-UBU-Q0178"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
