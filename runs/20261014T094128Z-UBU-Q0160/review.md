# Model-Committee Review

Run: `20261014T094128Z-UBU-Q0160`  
Question: `UBU-Q0160`  
Base commit: `1d8fa6da62f59386a23c30e7dfef667d056551c9`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0160-openai-mobile-trial-accounting-20261014`

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
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0160-openai-mobile-trial-accounting-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0160-openai-mobile-trial-accounting-20261014` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0160-openai-mobile-trial-accounting-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0160-openai-mobile-trial-accounting-20261014` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0160-openai-mobile-trial-accounting-20261014` | `openai` | `codex` | yes | 95 | Mechanical validation passes, and the patch modifies only allowed files. It usefully strengthens UBU-D0358 with matched workloads and cache conditions, frozen acceptance criteria, complete request accounting and separate accelerator/fallback latency populations. These requirements prevent omitted failures or CPU fallback from overstating accelerator benefit. Existing exact parity, named numeric tolerances, on-device CPU certification, provenance and recovery rules remain intact. UBU-Q0160 retains its metadata, Open status and Resolved by: None. The proposal appropriately advances the evidence protocol without claiming an API selection or hardware result. | None | The protocol still requires a working harness, declared acceptance thresholds and real-device measurements before an API can be selected. |
| `UBU-Q0160-openai-mobile-trial-accounting-20261014` | `openai` | `claude` | yes | 72 | Mechanical validation passes: patch applies, only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md are touched, and the UBU-Q0160 metadata line is preserved with Status: Open and Resolved by: None. The proposal is honest about what can be done while the question is hardware-blocked: it does not infer an API, threshold or passing configuration, and it says so in both the decision and the DESIGN.md text. The substantive addition closes a real evidentiary loophole (success-only latency populations, CPU fallback counted as accelerator benefit, failed ordinary trials reclassified as fault cohorts, retries not charged to the original deadline), which is a coherent extension of the UBU-D0283 parity rule and UBU-D0358 protocol. OPEN_QUESTIONS.md edits are minimal and correctly cross-reference the new decision without changing the resolution status. Score is held below the high range because the change is verbose relative to its new information content, overlaps existing §16.10.6 language, and does not move the question toward selection; it is a valid, auditable, low-risk refinement rather than a decisive contribution. | None | Marginal value is modest: UBU-Q0160 is blocked on physically connected device measurements, and this adds protocol text rather than evidence, so the question stays exactly as blocked as before., The DESIGN.md insertion is three dense paragraphs that partly restate obligations already present in §16.10.6 and UBU-D0358 (exact parity, fallback provenance, frozen same-device comparison), increasing section length and the chance of future drift between overlapping statements., "Positive minimum proportion of timely, parity-correct expected results" is intentionally unspecified; a later operator could satisfy it trivially, so the gate's strength depends on a future declaration the decision does not constrain., Decision numbering UBU-D0364 and the DECISIONS.md insertion point could not be independently verified against the base commit from this scoring context; the author's validation notes and the mechanical check are the only evidence. |

## Selected Summary

Require matched stewardship workloads and complete outcome accounting before selecting a mobile compute API; keep UBU-Q0160 Open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- "Positive minimum proportion of timely, parity-correct expected results" is intentionally unspecified; a later operator could satisfy it trivially, so the gate's strength depends on a future declaration the decision does not constrain.
- Decision numbering UBU-D0364 and the DECISIONS.md insertion point could not be independently verified against the base commit from this scoring context; the author's validation notes and the mechanical check are the only evidence.
- Marginal value is modest: UBU-Q0160 is blocked on physically connected device measurements, and this adds protocol text rather than evidence, so the question stays exactly as blocked as before.
- The DESIGN.md insertion is three dense paragraphs that partly restate obligations already present in §16.10.6 and UBU-D0358 (exact parity, fallback provenance, frozen same-device comparison), increasing section length and the chance of future drift between overlapping statements.
- The protocol still requires a working harness, declared acceptance thresholds and real-device measurements before an API can be selected.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T094128Z-UBU-Q0160/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T094128Z-UBU-Q0160/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T094128Z-UBU-Q0160"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
