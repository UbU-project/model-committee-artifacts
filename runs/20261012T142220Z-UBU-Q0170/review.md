# Model-Committee Review

Run: `20261012T142220Z-UBU-Q0170`  
Question: `UBU-Q0170`  
Base commit: `9d4645b3976c1a2c647d4ab57c91b318476546fb`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0170-openai-retention-defaults-01`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0170-openai-retention-defaults-01` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0170-openai-retention-defaults-01` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0170-openai-retention-defaults-01` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0170-openai-retention-defaults-01` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0170-openai-retention-defaults-01` | `openai` | `codex` | yes | 96 | Mechanical validation confirms clean application, allowed files and preserved question schema with Status: Solved. The proposal directly defines transient buffers, purposeful clip approval, local storage eligibility and nonrenewing expiry. It preserves append-only historical evidence, prevents derivative retention from bypassing policy, and distinguishes access expiry from confirmed deletion. Related questions remain open, and enforcement requirements are accurately presented as future gates. | None | The 60-second buffer ceiling and 24-hour clip default are policy choices requiring human approval., Enforcement depends on device/runtime persistence controls, reliable expiry tracking and interruption-safe cleanup; the proposal explicitly gates enablement on demonstrated behavior. |
| `UBU-Q0170-openai-retention-defaults-01` | `openai` | `claude` | yes | 84 | The patch passes mechanical validation, touches only the three expected documentation files, and directly answers the question: zero durable raw retention by default, a memory-only buffer with a 60-second acquisition-based ceiling, explicit version-bound approval for purposeful clips, encrypted local storage eligibility, an absolute 24-hour default expiry, and non-renewable deadlines. It explicitly separates raw-media deletion from append-only historical evidence and repeatedly states that automated enforcement is a gate, not a shipped claim, matching the 'Current direction' constraints. The OPEN_QUESTIONS.md hunk uses 'Status: Solved' with 'Resolved by: UBU-D0327' and preserves other metadata. Cross-references in the §21.1 contract and §21.1.5 gates are updated coherently so UBU-Q0170 is no longer listed as open there while UBU-Q0171 and UBU-Q0168 remain. requires_human_review is correctly set. Deductions are for the length and implementation-level detail of the normative text, unverifiable references to UBU-Q0177 and the §12.2 section number, and some overlap with the still-open operator-controls question. | Confirm at the base commit that UBU-Q0177 exists and concerns hardware/Device eligibility, and that the insertion point near UBU-D0292 is actually within DESIGN.md §12.2 so the §21.1 cross-references are accurate; adjust the references if not. | The 60-second buffer ceiling and 24-hour clip expiry are asserted policy numbers with no cited basis; they are flagged as human-approval ceilings, which is correct for a Security question with 'Human approval required', but a reviewer must still confirm they are acceptable defaults., Cross-references to UBU-Q0177 (described as hardware eligibility) and to '§12.2' as the home of the new text cannot be verified from the prompt; if either is wrong the new contract and §21.1.x pointers will be mislabeled., The DESIGN.md addition is dense and partly implementation-flavored (monotonic source clock, decoded-frame deadlines, swap/hibernation/crash-dump inventory, pre-reopen cleanup reconciliation). This is still framed as policy and gates rather than shipped behavior, but it raises audit burden and edges toward UBU-Q0171's operator-control and enforcement scope., '24 hours after the earliest captured sample' means later samples in a long clip get less than 24 hours; this is a deliberate anti-renewal choice but could surprise operators and should be understood at approval time., Deletion-status reporting (scheduled/pending/failed/confirmed) is specified here while detailed operator discard controls remain open in UBU-Q0171; the two decisions will need to stay consistent when Q0171 is resolved. |

## Selected Summary

Default consented continuous perception to memory-only raw media for at most 60 seconds; require explicit approval for encrypted local clips with a 24-hour default expiry.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- '24 hours after the earliest captured sample' means later samples in a long clip get less than 24 hours; this is a deliberate anti-renewal choice but could surprise operators and should be understood at approval time.
- Cross-references to UBU-Q0177 (described as hardware eligibility) and to '§12.2' as the home of the new text cannot be verified from the prompt; if either is wrong the new contract and §21.1.x pointers will be mislabeled.
- Deletion-status reporting (scheduled/pending/failed/confirmed) is specified here while detailed operator discard controls remain open in UBU-Q0171; the two decisions will need to stay consistent when Q0171 is resolved.
- Enforcement depends on device/runtime persistence controls, reliable expiry tracking and interruption-safe cleanup; the proposal explicitly gates enablement on demonstrated behavior.
- The 60-second buffer ceiling and 24-hour clip default are policy choices requiring human approval.
- The 60-second buffer ceiling and 24-hour clip expiry are asserted policy numbers with no cited basis; they are flagged as human-approval ceilings, which is correct for a Security question with 'Human approval required', but a reviewer must still confirm they are acceptable defaults.
- The DESIGN.md addition is dense and partly implementation-flavored (monotonic source clock, decoded-frame deadlines, swap/hibernation/crash-dump inventory, pre-reopen cleanup reconciliation). This is still framed as policy and gates rather than shipped behavior, but it raises audit burden and edges toward UBU-Q0171's operator-control and enforcement scope.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T142220Z-UBU-Q0170/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T142220Z-UBU-Q0170/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T142220Z-UBU-Q0170"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
