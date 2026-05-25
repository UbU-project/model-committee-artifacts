# Model-Committee Review

Run: `20260525T204413Z-UBU-Q0008`  
Question: `UBU-Q0008`  
Base commit: `620d9d0fb9994916928e61b2dcd8a58b2918406e`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0008-openai-codex-gpt5-001`

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
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0008-openai-codex-gpt5-001` |
| `work-score` | `claude` | `sonnet` | `ubu-q0008-openai-codex-gpt5-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0008-openai-codex-gpt5-001` |
| `work-score` | `claude` | `sonnet` | `ubu-q0008-openai-codex-gpt5-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0008-openai-codex-gpt5-001` | `openai-codex` | `codex` | yes | 94 | The patch cleanly resolves all nine parts of UBU-Q0008 with a Phase 1-friendly explicit assignment model, lease/heartbeat handling, observer-vs-executor semantics, no competitive claiming, logged lifecycle transitions, worker rejection, clarification requests, and mutation-request-only child Task proposals. It updates only the expected design, decision, and open question files, preserves the selected question schema, and leaves related retry, mutation, parent/child, and Delegation Substrate details to their existing questions. | None | Adds a new log event type and assignment status vocabulary that future schema work will need to keep synchronized., The distinction between offered, accepted, delivered, completed, and Task status is clear enough for Phase 1 but will still need implementation-level invariants. |
| `ubu-q0008-openai-codex-gpt5-001` | `openai-codex` | `claude` | yes | 89 | Mechanical validation confirms clean application. All nine sub-questions from UBU-Q0008 are addressed: explicit parent-directed assignment (Q1), polling-only-from-assigned-inbox (Q2), observation-without-execution-authority (Q3), single-lease-per-executor-slot with competitive claiming deferred (Q4), heartbeat expiry handling (Q5), logged transitions via new worker_assignment_updated event type (Q6), rejection with reason (Q7), clarification requests (Q8), and child-task proposals only through authorized mutation requests (Q9). OPEN_QUESTIONS.md preserves the single-line metadata format, updating only Status, Resolved-by, and Resolution fields. The decision record UBU-D0158 maps cleanly to the design section 24.1.2 added in DESIGN.md. Scope is appropriately bounded: retry construction, mutation request schema, Delegation Substrate, and parent/child automation shape are explicitly left to their dedicated open questions. The new log event type is added to the existing event-type list with a consistent naming pattern. No unnecessary files are touched and no extraneous abstractions are introduced. Minor deductions for unspecified lease timing mechanics and the implicit section renumbering risk. | None | The 10-status assignment FSM (offered, accepted, in_progress, clarification_requested, delivered, completed, rejected_by_worker, cancelled, expired, failed) adds implementation surface area; the distinction between 'delivered' and 'completed' requires careful enforcement to avoid status drift., Lease/heartbeat deadline semantics are referenced but not defined (duration, renewal protocol, clock-skew tolerance), which may cause ambiguity when UBU-Q0020 implements retry construction., Late-submission handling after expiration ('must pass expected-prior-version, idempotency, authority, and review checks') is described at a policy level only; the mechanism for these checks is not specified and could diverge from canonical review workflows., Section 24.1.3 is renumbered implicitly by inserting 24.1.2 before the existing model-committee worker text; if other documents cross-reference the prior section numbering this could introduce inconsistency. |

## Selected Summary

Resolve the Worker Assignment Model by making Phase 1 worker discovery parent-directed explicit assignment with lease-based lifecycle state, logged transitions, rejection, clarification, expiration, and mutation-request-only child Task proposals.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Adds a new log event type and assignment status vocabulary that future schema work will need to keep synchronized.
- Late-submission handling after expiration ('must pass expected-prior-version, idempotency, authority, and review checks') is described at a policy level only; the mechanism for these checks is not specified and could diverge from canonical review workflows.
- Lease/heartbeat deadline semantics are referenced but not defined (duration, renewal protocol, clock-skew tolerance), which may cause ambiguity when UBU-Q0020 implements retry construction.
- Section 24.1.3 is renumbered implicitly by inserting 24.1.2 before the existing model-committee worker text; if other documents cross-reference the prior section numbering this could introduce inconsistency.
- The 10-status assignment FSM (offered, accepted, in_progress, clarification_requested, delivered, completed, rejected_by_worker, cancelled, expired, failed) adds implementation surface area; the distinction between 'delivered' and 'completed' requires careful enforcement to avoid status drift.
- The distinction between offered, accepted, delivered, completed, and Task status is clear enough for Phase 1 but will still need implementation-level invariants.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260525T204413Z-UBU-Q0008/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260525T204413Z-UBU-Q0008/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260525T204413Z-UBU-Q0008"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
