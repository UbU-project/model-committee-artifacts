# Model-Committee Review

Run: `20260525T051634Z-UBU-Q0060`  
Question: `UBU-Q0060`  
Base commit: `5ebbb1d00b97d4513ca0c3289991ee01b5fd313e`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0060-001`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0060-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0060-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0060-001` | `codex` | `claude` | yes | 89 | The patch applies cleanly and is mechanically valid. It directly and comprehensively addresses all seven subquestions in UBU-Q0060: provider descriptor fields and classes (Q1/Q2), Compartment hard gates for no_cloud_llm/no_external_export/local_only (Q3), ContextBundle-based context minimization and redaction (Q4), BYOK credential scoping rules (Q5), UbUCorp managed inference as a non-privileged provider class (Q6), and disclosure requirements before cloud execution (Q7). The decision record UBU-D0157 is well-structured with status, consequences, and downstream refs. OPEN_QUESTIONS.md changes correctly update only the Status and Resolution fields while preserving all other metadata fields and the schema. No new questions are added and no files outside the allowed set are touched. The main concerns are forward references to ContextBundle governance (not yet fully resolved) and the hard-gate language being potentially too absolute for edge cases not yet modeled, but neither rises to the level of a required fix for Phase 1 architecture documentation. | None | DESIGN.md addition references '§2.11' by number, but section numbering cannot be verified without reading the full current file; a stale or wrong section reference would be misleading., ContextBundle is referenced as an established concept with specific required fields, but it may not yet be fully resolved upstream; this creates a forward dependency that could require revision if ContextBundle governance changes., The absolute hard-gate language ('A capability grant, provider preference, workflow setting, or user click cannot override these denials') is architecturally correct but may conflict with legitimate edge cases (e.g., emergency override by a privileged Identity) that are not yet modeled., UBU-Q0079, UBU-Q0080, UBU-Q0083, and UBU-Q0084 are listed as dependents but remain Open; if their resolution conflicts with the adapter interface or route envelope defined here, backtracking through this decision will be needed., The credential-scoping model specifies storage targets (local instance, OS/device secret store, user-owned worker) at a level of detail that may not survive Phase 1 implementation without requiring an amendment decision. |

## Selected Summary

Resolve UBU-Q0060 by adding a minimum provider-neutral LLM routing model with provider descriptors, route requests/decisions, Compartment hard gates, BYOK credential scoping, UbUCorp managed-inference boundaries, and cloud-use disclosure requirements.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- ContextBundle is referenced as an established concept with specific required fields, but it may not yet be fully resolved upstream; this creates a forward dependency that could require revision if ContextBundle governance changes.
- DESIGN.md addition references '§2.11' by number, but section numbering cannot be verified without reading the full current file; a stale or wrong section reference would be misleading.
- The absolute hard-gate language ('A capability grant, provider preference, workflow setting, or user click cannot override these denials') is architecturally correct but may conflict with legitimate edge cases (e.g., emergency override by a privileged Identity) that are not yet modeled.
- The credential-scoping model specifies storage targets (local instance, OS/device secret store, user-owned worker) at a level of detail that may not survive Phase 1 implementation without requiring an amendment decision.
- UBU-Q0079, UBU-Q0080, UBU-Q0083, and UBU-Q0084 are listed as dependents but remain Open; if their resolution conflicts with the adapter interface or route envelope defined here, backtracking through this decision will be needed.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260525T051634Z-UBU-Q0060/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260525T051634Z-UBU-Q0060/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260525T051634Z-UBU-Q0060"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
