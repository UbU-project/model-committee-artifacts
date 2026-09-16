# Model-Committee Review

Run: `20260916T181001Z-UBU-Q0145`  
Question: `UBU-Q0145`  
Base commit: `ab795e369ff46df40fd3077a4b0dce210e818ce0`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0145-gpt5-codex-20260916`

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
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0145-gpt5-codex-20260916` |
| `work-score` | `claude` | `sonnet` | `ubu-q0145-gpt5-codex-20260916` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0145-gpt5-codex-20260916` |
| `work-score` | `claude` | `sonnet` | `ubu-q0145-gpt5-codex-20260916` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0145-gpt5-codex-20260916` | `openai` | `codex` | yes | 92 | The proposal is clean, minimal, and directly resolves UBU-Q0145 with a scoped worker_request/worker_result protocol over existing sync statements. It preserves the OPEN_QUESTIONS metadata format, modifies only the expected decision/question files, and keeps workers from becoming an independent authority or admitted-state mutation path. The scope is appropriately narrow for Phase 2 while still useful for planning, simulation, summarization, previews, and candidate mutations. | None | The decision defines protocol fields at an architectural level but leaves exact wire/schema validation details for later implementation., The phrase "worker_result statements" inside the worker_result contents is slightly recursive/ambiguous and may want tightening in a later schema pass. |
| `ubu-q0145-gpt5-codex-20260916` | `openai` | `claude` | yes | 86 | Mechanical validation confirms the patch applies cleanly and only touches the allowed files (OPEN_QUESTIONS.md, DECISIONS.md). The OPEN_QUESTIONS.md edit preserves the single-line metadata schema, correctly flips Status to Solved, and adds a Resolved by reference to the new decision, matching the existing convention. The new DECISIONS.md entry (UBU-D0250) is scoped tightly to the §20 boundary referenced in the question: workers get a policy-checked scoped request/result exchange over ordinary sync statements, produce only candidate artifacts, and can never bypass normal admission. This avoids introducing a second authority/mutation path, which is the key architectural risk for worker protocols. The proposal is a single coherent resolution rather than a decomposition, so decomposition quality is not applicable. Scope is appropriately narrow — no unrelated files or speculative future-Device features are touched, and requires_human_review is honestly flagged given the architectural nature of the decision. | None | Introduces a new decision (UBU-D0250) whose interaction with existing admission/conflict/policy decisions is asserted but not cross-checked against those decisions' exact text in this patch, Retention/deletion-confirmation requirements for worker_result are asserted but not tied to a specific existing deletion-propagation decision, so downstream Phase 2 work must still verify consistency |

## Selected Summary

Resolve UBU-Q0145 by defining the minimum Phase 2 worker Device protocol as scoped worker_request and worker_result sync statements, with workers limited to computation and candidate output while normal admission remains the only path to admitted state.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DECISIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Introduces a new decision (UBU-D0250) whose interaction with existing admission/conflict/policy decisions is asserted but not cross-checked against those decisions' exact text in this patch
- Retention/deletion-confirmation requirements for worker_result are asserted but not tied to a specific existing deletion-propagation decision, so downstream Phase 2 work must still verify consistency
- The decision defines protocol fields at an architectural level but leaves exact wire/schema validation details for later implementation.
- The phrase "worker_result statements" inside the worker_result contents is slightly recursive/ambiguous and may want tightening in a later schema pass.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260916T181001Z-UBU-Q0145/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260916T181001Z-UBU-Q0145/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260916T181001Z-UBU-Q0145"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
