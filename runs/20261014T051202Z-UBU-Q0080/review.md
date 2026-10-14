# Model-Committee Review

Run: `20261014T051202Z-UBU-Q0080`  
Question: `UBU-Q0080`  
Base commit: `6a3aae470f2a51591f560d8a0548ef1211d38c49`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0080-delegation-mvp`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 82.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `codex-ubu-q0080-delegation-mvp` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `codex-ubu-q0080-delegation-mvp` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0080-delegation-mvp` | `codex` | `claude` | yes | 82 | Patch applies, touches only the three allowed files, and uses Status: Solved with Resolved by: UBU-D0342, so schema and allowlist pass. It directly answers all five subquestions: Task-versus-packet ownership (§21.5.1), the full executor type list with Phase 1 limits (§21.5.3), authority/evidence/privacy/output/review/deadline/escalation fields (§21.5.2 table), self-reminder semantics without handoff (§21.5.3), and the Phase 1 dogfooding slice with acceptance fixtures (§21.5.4). The decision record is scoped as a data-model contract and explicitly defers General Contractor, Association, and marketplace semantics, which matches the question's current direction. Deductions are for unverifiable cross-references, the removal of the prior status vocabulary without visible reconciliation elsewhere, and a schema that is heavier and denser than 'minimum' suggests. No blocking fix is required; a human reviewer should confirm the referenced decisions and sections exist and that no other text depends on the removed packet statuses. | None | Replaces the existing packet status vocabulary (proposed, offered, accepted, in_progress, delivered, reviewed, rejected, completed) with four preparation-only states and relocates execution state to the §25.1.2 assignment record; any other DESIGN.md or DECISIONS.md text that references the removed packet statuses would become inconsistent, and this cannot be verified from the prompt., Relies on many cross-references (UBU-D0137, UBU-D0074, UBU-D0157, UBU-D0177, §2.11, §3.10, §9.3, §21.2, §21.4, §25.1.2, §25.1.3, §27.3) whose existence and meaning are not verifiable here; the validation notes confirm ID uniqueness but not that these referenced sections and decisions say what the proposal assumes., The 'minimum' schema is fairly heavy: every table row is required on an admitted packet and the proposal introduces roughly fifteen new field names (execution_brief, schema_version, packet_version, expected_task_version, capability_grant_refs, originating_context_bundle_refs, route_decision_refs, execution_limits, idempotency_key, supersedes_packet_ref, etc.). This expands data-model surface in a question that is flagged as human-approval-required., Introduces `security_level: low` as a classification label for un-compartmented content; if that label is not already part of the privacy vocabulary it adds a new term without defining it., Dense prose with many nested rules (approval binding, revision invalidation, retirement semantics) makes auditing harder than a shorter contract would; some of this detail may belong in later implementation decisions rather than the MVP data-model answer. |

## Selected Summary

Define optional Task execution briefs and versioned DelegationPackets for solo clarity, explicit worker handoff, and Phase 1 GitHub dogfooding.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Dense prose with many nested rules (approval binding, revision invalidation, retirement semantics) makes auditing harder than a shorter contract would; some of this detail may belong in later implementation decisions rather than the MVP data-model answer.
- Introduces `security_level: low` as a classification label for un-compartmented content; if that label is not already part of the privacy vocabulary it adds a new term without defining it.
- Relies on many cross-references (UBU-D0137, UBU-D0074, UBU-D0157, UBU-D0177, §2.11, §3.10, §9.3, §21.2, §21.4, §25.1.2, §25.1.3, §27.3) whose existence and meaning are not verifiable here; the validation notes confirm ID uniqueness but not that these referenced sections and decisions say what the proposal assumes.
- Replaces the existing packet status vocabulary (proposed, offered, accepted, in_progress, delivered, reviewed, rejected, completed) with four preparation-only states and relocates execution state to the §25.1.2 assignment record; any other DESIGN.md or DECISIONS.md text that references the removed packet statuses would become inconsistent, and this cannot be verified from the prompt.
- The 'minimum' schema is fairly heavy: every table row is required on an admitted packet and the proposal introduces roughly fifteen new field names (execution_brief, schema_version, packet_version, expected_task_version, capability_grant_refs, originating_context_bundle_refs, route_decision_refs, execution_limits, idempotency_key, supersedes_packet_ref, etc.). This expands data-model surface in a question that is flagged as human-approval-required.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T051202Z-UBU-Q0080/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T051202Z-UBU-Q0080/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T051202Z-UBU-Q0080"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
