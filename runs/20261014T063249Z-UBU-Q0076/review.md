# Model-Committee Review

Run: `20261014T063249Z-UBU-Q0076`  
Question: `UBU-Q0076`  
Base commit: `afb7d1a9290fbf8ec346e04b59331226d139d2fa`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-6-UBU-Q0076-20261014`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 84.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-gpt-6-UBU-Q0076-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-gpt-6-UBU-Q0076-20261014` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-gpt-6-UBU-Q0076-20261014` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-6-UBU-Q0076-20261014` | `openai` | `codex` | no | n/a | codex exited with 1 | None | None |
| `openai-gpt-6-UBU-Q0076-20261014` | `openai` | `claude` | yes | 84 | Patch applies cleanly, touches only the four allowed files, and sets `Status: Solved` with `Resolved by: UBU-D0349` while preserving the single-line metadata and existing scores. All six subquestions are answered concretely: a seven-elapsed-day detailed default and unattended upper bound, a bounded future dependency frontier, explicit fragility criteria for early preparation, routing of wider horizons to explicit-override local/worker/premium paths with research deferrals, and a zero default discount with accepted timing-Preference overrides. The kernel contract note is appropriate rather than scope creep: it pins down that current scoring is unchanged and that placement-dependent discounting needs a versioned extension, which keeps the policy honest about what the kernel can express. The decision entry and the Resolution text are consistent with the DESIGN.md content and hedge correctly that this is accepted policy with required fixtures, not implemented behavior. Deductions are for verbosity and auditability of the DESIGN.md block, unverified section/decision anchors, and a few specific numeric claims about the contract that must match existing text. | None | Cross-references to DESIGN.md sections (§16.7, §16.10.6, §15.2.2, §16.3, §16.8) and to decisions UBU-D0128, UBU-D0282, UBU-D0289 could not be verified from the prompt; a stale or wrong anchor would weaken auditability., UBU-D0349 is assumed to be the next free decision ID; a collision with a concurrently authored decision would require renumbering across all four files., The PLANNING_KERNEL_CONTRACT.md note cites a default 256-Task ceiling and `priority = 1.0`; if those values do not match the current contract text the note introduces a contradiction., The DESIGN.md insertion is eight dense paragraphs under a bolded lead-in rather than a numbered subsection, which makes later citation and review harder., The policy adds new provenance/inspector reporting obligations (requested vs effective interval, reduction reason, policy source) that are adjacent to but not strictly asked by the question; modest scope growth., Resolves a Research-priority, human-approval-required question in one step; the fixture list is specified but not run, so enablement remains unproven (correctly disclosed by the proposal). |

## Selected Summary

Refine the provisional horizon policy into a seven-day detailed default, bounded future dependency inspection, evidence-based early preparation and explicit timing-Preference overrides.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`
- `PLANNING_KERNEL_CONTRACT.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Cross-references to DESIGN.md sections (§16.7, §16.10.6, §15.2.2, §16.3, §16.8) and to decisions UBU-D0128, UBU-D0282, UBU-D0289 could not be verified from the prompt; a stale or wrong anchor would weaken auditability.
- Resolves a Research-priority, human-approval-required question in one step; the fixture list is specified but not run, so enablement remains unproven (correctly disclosed by the proposal).
- The DESIGN.md insertion is eight dense paragraphs under a bolded lead-in rather than a numbered subsection, which makes later citation and review harder.
- The PLANNING_KERNEL_CONTRACT.md note cites a default 256-Task ceiling and `priority = 1.0`; if those values do not match the current contract text the note introduces a contradiction.
- The policy adds new provenance/inspector reporting obligations (requested vs effective interval, reduction reason, policy source) that are adjacent to but not strictly asked by the question; modest scope growth.
- UBU-D0349 is assumed to be the next free decision ID; a collision with a concurrently authored decision would require renumbering across all four files.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T063249Z-UBU-Q0076/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T063249Z-UBU-Q0076/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T063249Z-UBU-Q0076"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
