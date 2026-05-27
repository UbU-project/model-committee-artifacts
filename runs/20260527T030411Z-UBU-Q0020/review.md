# Model-Committee Review

Run: `20260527T030411Z-UBU-Q0020`  
Question: `UBU-Q0020`  
Base commit: `b0d71c9e540db17ee7e78c22ff07ba71b0e39ecd`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0020-retry-semantics`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 91.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0020-retry-semantics` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0020-retry-semantics` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0020-retry-semantics` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0020-retry-semantics` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0020-retry-semantics` | `openai-codex` | `codex` | yes | 92 | The proposal directly answers all parts of UBU-Q0020: failed attempts are preserved, retry siblings are created, policy precedence is defined, logging uses existing event surfaces, retry caps and nonretryable classes prevent loops, and worker failure feeds derived risk reports. The patch is limited to the expected documentation files, applies cleanly, tombstones the open question with a decision reference, and keeps adjacent open questions scoped appropriately. | None | Some retry metadata fields are introduced textually without confirming whether they already exist in the Task schema or need explicit schema entries elsewhere., The distinction between worker-created child Tasks and Phase 1 workers not directly creating canonical child Tasks is handled, but implementers may still need clearer terminology around attempt Tasks versus underlying Tasks. |
| `codex-ubu-q0020-retry-semantics` | `openai-codex` | `claude` | yes | 90 | The patch applies cleanly and directly answers all five sub-questions of UBU-Q0020. (1) Failed attempt plus retry sibling is chosen, consistent with the stated current leaning and justified by auditability. (2) A clear four-level policy hierarchy is specified: Task/Delegation Substrate packet → parent Container/Objective → worker/integration default → global Phase 1 default. (3) Logging reuses existing event surfaces (task_failed, worker_assignment_updated, worker_mutation_* family, recalculation_triggered) without introducing new top-level event types. (4) Infinite-loop prevention is multi-layered: retry-root attempt cap (three total), idempotency keys, expected-prior-version checks, lease checks, failure-class non-retryability, and mandatory human review after cap exhaustion. (5) Worker failure state feeds worker_or_automation_bottleneck and downstream risk reports. DESIGN.md and DECISIONS.md are mutually consistent, and the new decision record UBU-D0187 is self-contained with clear consequences. The tombstone format in OPEN_QUESTIONS.md is appropriate for a resolved question: Status and Resolved-by fields are present. The only notable gaps are prose-only field definitions for retry linkage metadata (no schema artifact) and the unconfirmed existence of the worker_mutation_* event names — both are deferred implementation concerns rather than design errors. No unnecessary scope is introduced, no new open questions are added, and UBU-Q0021 correctly remains open. | None | Retry-sibling linkage fields (retry_of_task_ref, retry_root_task_ref, attempt_number, retry_policy_summary, source_failure_log_refs) are named in prose but not yet formally defined in any schema artifact; implementations could interpret field semantics divergently until a schema patch arrives., The Phase 1 default cap of three total attempts is stated only in prose; without a machine-readable policy record or schema-level constraint there is no enforcement guarantee across implementations., Event type worker_mutation_submitted / worker_mutation_applied / worker_mutation_rejected are referenced as existing surfaces but are not confirmed to be defined elsewhere in this patch set; if they are new names, they require a matching schema entry., Interaction with UBU-Q0021 (automation parent/child structure) is acknowledged but underspecified: if Q0021 resolves with a different lineage model the retry_root / retry_of lineage fields defined here may need retrofitting., The OPEN_QUESTIONS.md tombstone strips the full question body, replacing it with a single collapsed metadata line plus a forward reference; future readers cannot reconstruct intent from OPEN_QUESTIONS.md alone without consulting DECISIONS.md or git history. |

## Selected Summary

Resolve Automation Worker retry semantics by preserving failed attempts, creating retry sibling Tasks, defining retry-policy precedence, logging retry chains through existing event surfaces, capping automatic retries, and feeding worker retry state into derived risk reports.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Event type worker_mutation_submitted / worker_mutation_applied / worker_mutation_rejected are referenced as existing surfaces but are not confirmed to be defined elsewhere in this patch set; if they are new names, they require a matching schema entry.
- Interaction with UBU-Q0021 (automation parent/child structure) is acknowledged but underspecified: if Q0021 resolves with a different lineage model the retry_root / retry_of lineage fields defined here may need retrofitting.
- Retry-sibling linkage fields (retry_of_task_ref, retry_root_task_ref, attempt_number, retry_policy_summary, source_failure_log_refs) are named in prose but not yet formally defined in any schema artifact; implementations could interpret field semantics divergently until a schema patch arrives.
- Some retry metadata fields are introduced textually without confirming whether they already exist in the Task schema or need explicit schema entries elsewhere.
- The OPEN_QUESTIONS.md tombstone strips the full question body, replacing it with a single collapsed metadata line plus a forward reference; future readers cannot reconstruct intent from OPEN_QUESTIONS.md alone without consulting DECISIONS.md or git history.
- The Phase 1 default cap of three total attempts is stated only in prose; without a machine-readable policy record or schema-level constraint there is no enforcement guarantee across implementations.
- The distinction between worker-created child Tasks and Phase 1 workers not directly creating canonical child Tasks is handled, but implementers may still need clearer terminology around attempt Tasks versus underlying Tasks.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260527T030411Z-UBU-Q0020/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260527T030411Z-UBU-Q0020/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260527T030411Z-UBU-Q0020"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
