# Model-Committee Review

Run: `20261012T143320Z-UBU-Q0171`  
Question: `UBU-Q0171`  
Base commit: `a3843f72dab3ce719eb0a533a57d1098d4b041ed`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-UBU-Q0171-operator-controls-20261012`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 91.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-UBU-Q0171-operator-controls-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-UBU-Q0171-operator-controls-20261012` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-UBU-Q0171-operator-controls-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-UBU-Q0171-operator-controls-20261012` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-UBU-Q0171-operator-controls-20261012` | `openai` | `codex` | yes | 97 | Reported mechanical validation confirms clean application, allowed-file scope and question-schema preservation. The proposal directly specifies inspection and independent discard, summary, hash, clip and export controls, including retention effects, inherited source restrictions, separate export authority, stale-approval invalidation and minimized correction/deletion history. It preserves hard denials and distinguishes design resolution from implemented enforcement. No substantive required fixes are evident. | None | Enforcement remains unverified; the required revision-pinned behavioral gates must pass before controls or export routes are enabled. |
| `openai-UBU-Q0171-operator-controls-20261012` | `openai` | `claude` | yes | 86 | Mechanical validation passes: the patch applies, only the three allowlisted files change, and the UBU-Q0171 metadata line stays single-line with Status: Solved and Resolved by: UBU-D0328, which is the correct schema. Hunk offsets are internally consistent (20 added lines in §12.2 shift the two later DESIGN.md hunks by exactly 20; the DECISIONS.md append and OPEN_QUESTIONS.md edit match their headers). The work is directly responsive to the question: it specifies capture state and pause/stop independent of model completion, a policy-filtered inventory showing destination, retention rule and cleanup state, independent discard/summary/hash/clip/export actions with nothing selected by default, hard local_only/no_cloud_llm/no_external_export denials carried onto derivatives and every egress route, and preserved minimized correction/deletion/gap metadata. It explicitly closes the two traps the Current direction names: hashes and summaries inherit source restrictions and can leak correlation, and raw deletion never grants export authority. The DECISIONS.md entry follows the UBU-D0327 pattern of revision-pinned behavioral gates stated as required fixtures rather than passing results, and the proposal correctly flags human review for a Security decision. Cross-reference updates in DESIGN.md (UBU-D0292 paragraph, retention paragraph, §21.1 contract line, §21.1.5 gates line) are the minimal set needed so the living document no longer says UBU-Q0171 is open. Deductions are for density and auditability of the new block, unverifiable section anchors, and the stale pointer left in the UBU-D0327 decision text. No decomposition was attempted, which is appropriate for a single design-contract question. | Before merge, confirm DESIGN.md §1, §17.0 and §§21.3–21.4 exist at base commit a3843f7 and cover what the new text attributes to them; retarget the references if not. | The new DESIGN.md §12.2 block is very dense (five long paragraphs plus a five-row table). It is hard to audit line by line and adds materially to prompt size for later committee rounds., The new text cites DESIGN.md §1, §17.0 and §§21.3–21.4 as still applying. Those anchors are not visible in the patch context and could not be verified from this prompt; a dangling section reference would be a real defect in a design contract., DECISIONS.md UBU-D0327 still reads '`UBU-Q0171` retains detailed operator controls'. Leaving historical decisions untouched is defensible, but the pointer is now stale and a reader of DECISIONS.md alone will not see that UBU-D0328 superseded that sentence., The question's Current direction points at 'Delta §17.9' while the design text lands in §12.2 next to UBU-D0327. This is consistent with where UBU-D0327 was placed, but the mapping from the delta section to §12.2 is implicit., Export specifics (idempotency key, per-segment recheck on streamed export, recipient retention assurances) go deeper than the question strictly requires. They are in-domain and framed as a contract rather than implementation, so this is detail scope, not file scope. |

## Selected Summary

Define operator inspection, independent AV artifact actions and separately approved, policy-gated export.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- DECISIONS.md UBU-D0327 still reads '`UBU-Q0171` retains detailed operator controls'. Leaving historical decisions untouched is defensible, but the pointer is now stale and a reader of DECISIONS.md alone will not see that UBU-D0328 superseded that sentence.
- Enforcement remains unverified; the required revision-pinned behavioral gates must pass before controls or export routes are enabled.
- Export specifics (idempotency key, per-segment recheck on streamed export, recipient retention assurances) go deeper than the question strictly requires. They are in-domain and framed as a contract rather than implementation, so this is detail scope, not file scope.
- The new DESIGN.md §12.2 block is very dense (five long paragraphs plus a five-row table). It is hard to audit line by line and adds materially to prompt size for later committee rounds.
- The new text cites DESIGN.md §1, §17.0 and §§21.3–21.4 as still applying. Those anchors are not visible in the patch context and could not be verified from this prompt; a dangling section reference would be a real defect in a design contract.
- The question's Current direction points at 'Delta §17.9' while the design text lands in §12.2 next to UBU-D0327. This is consistent with where UBU-D0327 was placed, but the mapping from the delta section to §12.2 is implicit.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T143320Z-UBU-Q0171/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T143320Z-UBU-Q0171/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T143320Z-UBU-Q0171"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
