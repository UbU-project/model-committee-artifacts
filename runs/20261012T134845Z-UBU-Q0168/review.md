# Model-Committee Review

Run: `20261012T134845Z-UBU-Q0168`  
Question: `UBU-Q0168`  
Base commit: `a0931cd581db4790bf155ea18c0cf9629d9c91c8`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-6-ubu-q0168-latency-contract-20261012`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 80.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-gpt-6-ubu-q0168-latency-contract-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-gpt-6-ubu-q0168-latency-contract-20261012` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-gpt-6-ubu-q0168-latency-contract-20261012` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-6-ubu-q0168-latency-contract-20261012` | `openai` | `codex` | no | n/a | codex exited with 1 | None | None |
| `openai-gpt-6-ubu-q0168-latency-contract-20261012` | `openai` | `claude` | yes | 80 | Mechanical validation passed (applies cleanly, allowlist of DECISIONS.md/DESIGN.md/OPEN_QUESTIONS.md, question schema intact). The patch directly answers the question's core ask — which latency classes exist and how each degrades — by defining snapshot_capture, conversational, streamed_supervision and retrospective with separate acknowledgment / first-useful-result / total-job / context-age / stream-gap ceilings, monotonic-clock deadlines, freshness-and-authority binding, offline/battery/thermal degradation ordering, and interruption semantics that invalidate pending bindings locally and forbid late results from overwriting corrections or triggering duplicate effects. It correctly keeps capture latency, planner streaming and proof of execution distinct, as the question's current direction requires, and it defers all numeric ceilings and hardware claims to a revision-pinned measurement gate rather than inventing numbers. Keeping Q0168 at Status: Open with an updated 'Current direction' and 'Resolution' is a conservative, schema-valid choice consistent with the question's own instruction that deadlines come from measured local-device evidence; the single-line metadata is preserved byte-for-byte. The decision block follows the house format used by UBU-D0324 (Status → DESIGN.md anchor, explicit non-claims). Deductions are for unverifiable cross-references, the Q0177/Q0183 hardware attribution ambiguity, the untouched Delta §17.6 pointer, and the absence of a machine-visible Resolved-by/narrowed-by link; none of these are blocking, and requires_human_review is correctly set given the question's human-approval eligibility. | Verify that UBU-Q0177 is the intended owner of 'hardware selection' and that it is distinct from UBU-Q0183's 'real hardware' / device-allocation role in §21.1.3; align wording or reference if they overlap., Confirm DESIGN.md §21.8, §16.5–§16.7 and PLANNING_KERNEL_CONTRACT.md §4 exist and cover durable checkpoints, cached-Plan freshness checks and CPU certification respectively., Optionally check Delta §17.6 for any statement that now conflicts with §21.1.4 and add a one-line pointer there if the repo convention expects it. | Cross-reference consistency is unverified from the prompt alone: the new text cites DESIGN.md §21.8 (durable checkpoint), §16.5–§16.7 (cached Plan legitimacy/envelope/freshness) and PLANNING_KERNEL_CONTRACT.md §4 (CPU certification); if any anchor is absent or means something different, the contract points at nothing., Possible ownership conflict: existing §21.1.3 says scenario results run on 'the real hardware selected by UBU-Q0183', while the new §21.1.4 and the Q0168 resolution text say hardware selection remains UBU-Q0177 ('secure hardware selection'). If these are not genuinely distinct concerns, the repo now attributes hardware selection to two different questions., The question stays Open with 'Resolved by: None' even though UBU-D0325 now answers its architectural half; the partial resolution is recorded only in prose, so tooling that tracks decision→question linkage will not see the narrowing., Delta §17.6 is still cited as the question's source direction but is not touched; if that section states a contract that differs from §21.1.4 the two now diverge., The §21.1.4 addition is dense (roughly 29 lines of long paragraphs plus a table) with many normative 'cannot/never/requires' statements; auditability is acceptable but reviewers must read it closely to catch any clause that overreaches into implementation or schema claims. On a read-through none does, and the decision explicitly disclaims numeric, hardware, safety-critical, kernel API and wire-schema changes. |

## Selected Summary

Define four interaction latency classes with bounded waiting, freshness checks, degradation and interruption rules; retain UBU-Q0168 for measured numerical profiles.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Cross-reference consistency is unverified from the prompt alone: the new text cites DESIGN.md §21.8 (durable checkpoint), §16.5–§16.7 (cached Plan legitimacy/envelope/freshness) and PLANNING_KERNEL_CONTRACT.md §4 (CPU certification); if any anchor is absent or means something different, the contract points at nothing.
- Delta §17.6 is still cited as the question's source direction but is not touched; if that section states a contract that differs from §21.1.4 the two now diverge.
- Possible ownership conflict: existing §21.1.3 says scenario results run on 'the real hardware selected by UBU-Q0183', while the new §21.1.4 and the Q0168 resolution text say hardware selection remains UBU-Q0177 ('secure hardware selection'). If these are not genuinely distinct concerns, the repo now attributes hardware selection to two different questions.
- The question stays Open with 'Resolved by: None' even though UBU-D0325 now answers its architectural half; the partial resolution is recorded only in prose, so tooling that tracks decision→question linkage will not see the narrowing.
- The §21.1.4 addition is dense (roughly 29 lines of long paragraphs plus a table) with many normative 'cannot/never/requires' statements; auditability is acceptable but reviewers must read it closely to catch any clause that overreaches into implementation or schema claims. On a read-through none does, and the decision explicitly disclaims numeric, hardware, safety-critical, kernel API and wire-schema changes.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T134845Z-UBU-Q0168/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T134845Z-UBU-Q0168/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T134845Z-UBU-Q0168"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
