# Model-Committee Review

Run: `20260526T003928Z-UBU-Q0036`  
Question: `UBU-Q0036`  
Base commit: `331d5bc36a090d9d20832846be48e1a79fb4ea87`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0036-001`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0036-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0036-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0036-001` | `codex` | `claude` | yes | 88 | The patch applies cleanly and is confirmed by mechanical validation. It directly answers UBU-Q0036 by specifying the minimum v0.1/v0.2 run-log directory structure (manifest.json, inputs/, schemas/, prompts/, raw/, parsed/, patches/, scores/, top-level selected artifacts), the required manifest fields, provider invocation record fields, security constraints (no secrets/keys), and review artifact content. The DESIGN.md addition (§3.1.2) and DECISIONS.md entry (UBU-D0160) are appropriately complementary rather than redundant—the design section is normative prose and the decision entry is the auditable record. OPEN_QUESTIONS.md is updated correctly: status changed to Solved, Resolved-by updated to include UBU-D0160, and the metadata remains on one line preserving the question schema. Scope is disciplined: only the three documentation files are touched, publication policy is explicitly deferred to UBU-Q0046, and no implementation files are modified. The decision is internally consistent with the current-direction text from the question itself, covering all the v0.2 extensions (Claude Code prompts, raw CLI JSON envelopes, structured_output payloads, invocation metadata, score-matrix entries, disagreement flags, quorum results, publication instructions). Minor deductions for vague field names and the solved/open tension noted in risks. | None | The manifest required-fields list is long and specific; implementations may find it overly prescriptive before the first working v0.2 run exists to validate feasibility., Marking Q0036 as 'Solved' while simultaneously noting the long-term format 'may evolve through schema migrations and later decisions' creates mild ambiguity about whether the question is truly closed or only partially resolved at the Phase 1 boundary., Some field descriptions in the manifest spec (e.g., 'provider configuration summaries') remain vague enough that different implementations could produce incompatible run directories without violating the written contract. |

## Selected Summary

Accept a manifest-indexed v0.1/v0.2 model-committee run-log format with required input snapshots, schemas, prompts, raw and parsed provider artifacts, patches, score matrices, quorum/disagreement records, selected artifacts, and redacted provider invocation metadata.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Marking Q0036 as 'Solved' while simultaneously noting the long-term format 'may evolve through schema migrations and later decisions' creates mild ambiguity about whether the question is truly closed or only partially resolved at the Phase 1 boundary.
- Some field descriptions in the manifest spec (e.g., 'provider configuration summaries') remain vague enough that different implementations could produce incompatible run directories without violating the written contract.
- The manifest required-fields list is long and specific; implementations may find it overly prescriptive before the first working v0.2 run exists to validate feasibility.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T003928Z-UBU-Q0036/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T003928Z-UBU-Q0036/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T003928Z-UBU-Q0036"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
