# Model-Committee Review

Run: `20260526T165034Z-UBU-Q0079`  
Question: `UBU-Q0079`  
Base commit: `958e7f087866079a8d05362c97db4e971b8890f0`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0079-mcp-capability-boundary`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

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
| `work-generate` | `ollama:sam860__deepseek-r1-0528-qwen3--8b` | `sam860/deepseek-r1-0528-qwen3:8b` | `n/a` |
| `work-generate` | `ollama:qooba__qwen3-coder-30b-a3b-instruct--q3_k_m` | `qooba/qwen3-coder-30b-a3b-instruct:q3_k_m` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0079-mcp-capability-boundary` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-generate` | `ollama:qooba__qwen3-coder-30b-a3b-instruct--q3_k_m` | `qooba/qwen3-coder-30b-a3b-instruct:q3_k_m` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0079-mcp-capability-boundary` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0079-mcp-capability-boundary` | `codex` | `claude` | yes | 89 | The patch applies cleanly per mechanical validation. It directly answers all five subquestions of UBU-Q0079: the Phase 1 tool set is enumerated, MCPToolCapabilityGrant fields are specified, the candidate-envelope pattern for write-like calls is defined, audit/deny/retry/rollback semantics are given, and the loopback dogfooding fixture is described. The OPEN_QUESTIONS.md change correctly converts the full question block to a compact tombstone with updated Status ('Solved'), updated 'Resolved by: UBU-D0177', and a pointer to the decision — preserving all header fields and their format. DECISIONS.md adds UBU-D0177 as the anchoring decision record with a full Consequences block. DESIGN.md §21.4 is updated with matching normative text. The proposal reuses existing worker-mutation, ContextBundle, Compartment, and Log primitives rather than creating a parallel authority model, which is architecturally consistent and reduces implementation surface. Compartment policy as a hard upper bound is clearly stated and specific denied flags are enumerated. No new open questions are added, no files outside the allowlist are modified, and no unnecessary scope is introduced. Minor issues — the inline caveat on association_attestation_candidate.submit, the unupdated Scored-from-commit field, and the forward reference to UBU-Q0080 — are insufficient to block acceptance but represent small hygiene improvements for a future pass. | None | association_attestation_candidate.submit is listed in the Phase 1 tool set with an inline conditional caveat ('for fixtures or manually reviewed Association-introspection dogfooding') rather than being cleanly separated or excluded; this could create ambiguity about whether it is a first-class Phase 1 tool or a special-case exception., The 'Scored from commit: None' field in the OPEN_QUESTIONS.md tombstone is not updated to the base commit hash (958e7f087866079a8d05362c97db4e971b8890f0), which may cause traceability gaps in the question audit trail., UBU-D0177 Consequences mention that UBU-Q0080 'can define Delegation Substrate fields on top of this candidate and capability boundary', but no corresponding open question is created or linked; if UBU-Q0080 does not yet exist this is a dangling forward reference., The MCPToolCapabilityGrant schema is defined in prose only; no normative schema artifact or DESIGN.md section reference for the grant structure is linked, which may create implementation ambiguity. |

## Selected Summary

Resolve UBU-Q0079 by making Phase 1 MCP tools narrow query/candidate surfaces governed by capability grants, Compartment hard denials, candidate admission, audit logs, retry/idempotency, append-only repair, and a local loopback dogfooding fixture.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- The 'Scored from commit: None' field in the OPEN_QUESTIONS.md tombstone is not updated to the base commit hash (958e7f087866079a8d05362c97db4e971b8890f0), which may cause traceability gaps in the question audit trail.
- The MCPToolCapabilityGrant schema is defined in prose only; no normative schema artifact or DESIGN.md section reference for the grant structure is linked, which may create implementation ambiguity.
- UBU-D0177 Consequences mention that UBU-Q0080 'can define Delegation Substrate fields on top of this candidate and capability boundary', but no corresponding open question is created or linked; if UBU-Q0080 does not yet exist this is a dangling forward reference.
- association_attestation_candidate.submit is listed in the Phase 1 tool set with an inline conditional caveat ('for fixtures or manually reviewed Association-introspection dogfooding') rather than being cleanly separated or excluded; this could create ambiguity about whether it is a first-class Phase 1 tool or a special-case exception.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T165034Z-UBU-Q0079/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T165034Z-UBU-Q0079/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T165034Z-UBU-Q0079"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
