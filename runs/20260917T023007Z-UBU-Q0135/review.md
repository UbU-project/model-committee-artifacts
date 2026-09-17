# Model-Committee Review

Run: `20260917T023007Z-UBU-Q0135`  
Question: `UBU-Q0135`  
Base commit: `50bebb3caae7d6a156b9210087ec3a7814eef9ac`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-5-codex-UBU-Q0135-20260917-01`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0135-20260917-01` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0135-20260917-01` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0135-20260917-01` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0135-20260917-01` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-5-codex-UBU-Q0135-20260917-01` | `openai` | `codex` | yes | 94 | The patch is valid after normalization, stays within the allowed documentation files, and directly answers all subquestions: authority scope and explicit denials, submission/result envelopes, admission path, failure and partial-result handling, provenance metadata, enforcement of no direct state mutation, and remote transport invariants. It builds cleanly on the existing backend trust boundary without expanding into implementation or unrelated remote-device design. Remaining risks are mostly precision risks around future concrete schema encoding, not blockers for an architecture decision. | None | The protocol is specified as normative documentation rather than exact serialized schema definitions, so implementation still has some field-shape discretion., It marks UBU-Q0135 solved while dependent broader worker-device questions remain open, though the proposal appropriately scopes those dependencies to later transport work. |
| `openai-gpt-5-codex-UBU-Q0135-20260917-01` | `openai` | `claude` | yes | 82 | The proposal directly answers all six subquestions: authority scope and denials, submission/result envelope and admission path, failure/timeout/malformed representation, provenance metadata, enforcement of no-direct-mutation, and invariant fields for remote transport substitution. It correctly builds on the existing UBU-D0254 boundary rather than re-deriving it, keeps the new decision narrowly scoped to Phase 1b local workers (deferring Phase 2 to UBU-Q0145), and only touches DECISIONS.md, DESIGN.md, and OPEN_QUESTIONS.md — consistent with a documentation-only decision resolution. OPEN_QUESTIONS.md edits preserve the existing single-line metadata schema format and update Status/Resolved by/Resolution fields correctly. The main flaw is mechanical: the raw patch is reported as corrupt at line 57 and only succeeds via --recount normalization, which is a genuine (if minor) authoring defect rather than a content problem. No other candidate proposals were provided to compare against. | Regenerate the patch with correct hunk line counts so it applies without --recount normalization, Consider whether Importance/Automation-likelihood/Risk scores should be filled in or left as TBD is acceptable practice | Patch only applied cleanly after --recount normalization, indicating a hunk-context/line-count mismatch in the raw diff that should be fixed at the source to avoid relying on lenient patch application, Introduces several new named types (LocalAdvisorySubmission, LocalAdvisoryResult) without corresponding schema/interface changes elsewhere in the repo, so the decision is more prose-specification than enforceable artifact until a schema file exists, Status set directly to 'Solved' with Answerability 100 while Importance/Automation-likelihood/Risk scores remain 'TBD', which is a slightly inconsistent metadata state for a fully resolved question |

## Selected Summary

Resolve UBU-Q0135 with a concrete Phase 1b local advisory worker protocol: scoped WorkerAuthority, local submission/result envelopes, failure/status handling, provider/model provenance, enforced no-write boundary, and remote-transport invariants.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Introduces several new named types (LocalAdvisorySubmission, LocalAdvisoryResult) without corresponding schema/interface changes elsewhere in the repo, so the decision is more prose-specification than enforceable artifact until a schema file exists
- It marks UBU-Q0135 solved while dependent broader worker-device questions remain open, though the proposal appropriately scopes those dependencies to later transport work.
- Patch only applied cleanly after --recount normalization, indicating a hunk-context/line-count mismatch in the raw diff that should be fixed at the source to avoid relying on lenient patch application
- Status set directly to 'Solved' with Answerability 100 while Importance/Automation-likelihood/Risk scores remain 'TBD', which is a slightly inconsistent metadata state for a fully resolved question
- The protocol is specified as normative documentation rather than exact serialized schema definitions, so implementation still has some field-shape discretion.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260917T023007Z-UBU-Q0135/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260917T023007Z-UBU-Q0135/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260917T023007Z-UBU-Q0135"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
