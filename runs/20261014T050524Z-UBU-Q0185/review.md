# Model-Committee Review

Run: `20261014T050524Z-UBU-Q0185`  
Question: `UBU-Q0185`  
Base commit: `1b55da79c2231ff329f026d81bef54847d45af6c`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0185-openai-gpt-6-runtime-policy`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0185-openai-gpt-6-runtime-policy` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0185-openai-gpt-6-runtime-policy` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0185-openai-gpt-6-runtime-policy` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0185-openai-gpt-6-runtime-policy` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0185-openai-gpt-6-runtime-policy` | `openai` | `codex` | yes | 96 | Reported mechanical validation passes, and changes remain within the three allowed files. The proposal directly resolves sampling-policy ownership while preserving UbU's grammar, resource, validation and admission responsibilities. It uses Status: Solved, preserves the remaining question metadata, retains existing review seed/ceiling behavior and avoids unsupported causal claims about the Vocabulary abort. Documentation and fixture requirements make the decision auditable without claiming implementation evidence. | None | Operator-owned sampling can vary across runtimes; adapter documentation, configuration qualification and conformance fixtures remain outstanding. |
| `UBU-Q0185-openai-gpt-6-runtime-policy` | `openai` | `claude` | yes | 82 | Patch applies cleanly, touches only the three allowlisted files, and passes mechanical schema validation. The question block uses Status: Solved with Resolved by: UBU-D0341 and leaves all other metadata fields untouched, which is correct. The proposal directly answers the architecture question (sampling policy is operator-owned in the runtime; UbU keeps grammar, resource, validation and admission contracts), is consistent with the existing 'deliberately-outside' evidence from UBU-D0302, correctly refuses to claim a cause for the Vocabulary abort, and sets requires_human_review in line with the question's human-approval requirement. The decision text is auditable and hedged appropriately about what it does not establish. Deductions are for verbosity and mild scope expansion: the DESIGN.md addition restates failure-path contracts rather than citing them, and the qualification-profile pinning requirement reaches into UBU-D0331/UBU-D0336 territory in a way that cannot be checked here. These are quality concerns, not blockers, so no fix is strictly required. | None | The decision adds new obligations to qualified Device/backend profiles (pinning sampling configuration or its digest, re-running qualification checks) by reference to UBU-D0331/UBU-D0336; those references are not verifiable from this prompt and could conflict with or silently extend what those decisions actually require., DESIGN.md §1 gains three dense paragraphs that restate contracts likely already specified elsewhere (saved-state preservation on abort/timeout, retry authority bindings, finite budgets). Duplicated normative text can drift from the canonical contract documents., The 'incompatible runtime leaves the affected operation unavailable with an explicit limitation and eligible retry/manual route' clause introduces a concrete behavioral commitment that goes somewhat beyond the ownership question itself and is asserted without pointing at an existing limitation-reporting mechanism., Fixture requirements (request inspection for absent sampler overrides, abort/timeout/invalid-output paths) are declared as conformance gates but no owning section or task is identified for them, so they may become orphaned requirements. |

## Selected Summary

Keep advisory sampling configuration in the operator-owned runtime; retain UbU-owned grammar, resource, validation and review contracts.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- DESIGN.md §1 gains three dense paragraphs that restate contracts likely already specified elsewhere (saved-state preservation on abort/timeout, retry authority bindings, finite budgets). Duplicated normative text can drift from the canonical contract documents.
- Fixture requirements (request inspection for absent sampler overrides, abort/timeout/invalid-output paths) are declared as conformance gates but no owning section or task is identified for them, so they may become orphaned requirements.
- Operator-owned sampling can vary across runtimes; adapter documentation, configuration qualification and conformance fixtures remain outstanding.
- The 'incompatible runtime leaves the affected operation unavailable with an explicit limitation and eligible retry/manual route' clause introduces a concrete behavioral commitment that goes somewhat beyond the ownership question itself and is asserted without pointing at an existing limitation-reporting mechanism.
- The decision adds new obligations to qualified Device/backend profiles (pinning sampling configuration or its digest, re-running qualification checks) by reference to UBU-D0331/UBU-D0336; those references are not verifiable from this prompt and could conflict with or silently extend what those decisions actually require.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T050524Z-UBU-Q0185/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T050524Z-UBU-Q0185/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T050524Z-UBU-Q0185"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
