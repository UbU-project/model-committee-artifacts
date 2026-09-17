# Model-Committee Review

Run: `20260917T115652Z-UBU-Q0136`  
Question: `UBU-Q0136`  
Base commit: `40cf4bb7e8b2f4549debd6c51df6612e111ffe78`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0136-20260917-openai-gpt5-codex-01`

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
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0136-20260917-openai-gpt5-codex-01` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0136-20260917-openai-gpt5-codex-01` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0136-20260917-openai-gpt5-codex-01` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0136-20260917-openai-gpt5-codex-01` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0136-20260917-openai-gpt5-codex-01` | `openai` | `codex` | yes | 93 | The proposal directly answers all six subquestions: secrets live outside canonical state, credentials are Compartment-labelled and brokered by the controller, revocation/rotation is versioned and checked at dispatch, worker credential categories are defined, reusable secret egress is forbidden and enforced by serialization/clearance filters, and exports/backups/diagnostics exclude secret material. It keeps scope to Phase 1b single-Device custody and explicitly avoids claiming cross-Device replication or encryption at rest. The patch is auditable, limited to the expected design/decision/question files, and mechanically valid. | None | The decision relies on future implementation of typed secret references, central serializers, and clearance filters; without follow-up implementation tickets/tests, enforcement could remain aspirational., The phrase "local secret store or equivalent operator-controlled local custody" is intentionally broad and may need tightening during implementation to avoid inconsistent single-Device storage choices. |
| `UBU-Q0136-20260917-openai-gpt5-codex-01` | `openai` | `claude` | yes | 89 | The proposal directly and comprehensively answers all six subquestions: token location relative to canonical store (Device-local secret capability, not canonical), Compartment labeling and cross-Compartment worker access (broadest-label rule, brokered use only), revocation/rotation semantics and effect on queued work (version recheck, cancel/block/retry), what counts as a credential (bright-line test distinguishing secrets from provenance metadata), enforcement of non-replication/non-logging at egress (typed secret references, clearance filters), and exclusion from exports/backups/diagnostics (redacted metadata only, reauthorization flag on restore). It correctly scopes itself to a single Device and explicitly defers cross-Device replication, encryption-at-rest, and rotation propagation to UBU-Q0149, matching the question's stated boundaries. The patch touches only OPEN_QUESTIONS.md, DESIGN.md, and DECISIONS.md, consistent with the project's established pattern for resolving open questions, and mechanical validation confirms it applies cleanly. The status-line and decision-block formatting mirrors existing conventions in the surrounding diff context. Minor risk is dependency on the correctness/consistency of the referenced prior decision UBU-D0252, which could not be independently verified from the provided diff alone, and a debatable classification of unauthenticated loopback endpoints as non-credential. | None | Relies on UBU-D0252 secret-capability semantics existing and being consistent with this extension; if UBU-D0252's definitions diverge, this decision could create subtle conflicts, Defines 'unauthenticated loopback endpoint' as non-credential provenance, which is a judgment call that could be contested depending on local threat model (e.g., loopback endpoints are not always safe from other local processes), Enforcement described as 'egress and serialization' clearance filters is architecturally sound but is a design commitment (typed secret reference, clearance filter) that later implementation could find hard to retrofit uniformly across all listed egress points |

## Selected Summary

Resolve UBU-Q0136 by defining Phase 1b single-Device credential custody as Device-local secret capabilities outside the canonical StateStore, with scoped metadata handles, controller-brokered use, revocation/rotation version checks, and egress enforcement for logs, workers, projections, exports, backups, and diagnostics.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DESIGN.md`
- `DECISIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Defines 'unauthenticated loopback endpoint' as non-credential provenance, which is a judgment call that could be contested depending on local threat model (e.g., loopback endpoints are not always safe from other local processes)
- Enforcement described as 'egress and serialization' clearance filters is architecturally sound but is a design commitment (typed secret reference, clearance filter) that later implementation could find hard to retrofit uniformly across all listed egress points
- Relies on UBU-D0252 secret-capability semantics existing and being consistent with this extension; if UBU-D0252's definitions diverge, this decision could create subtle conflicts
- The decision relies on future implementation of typed secret references, central serializers, and clearance filters; without follow-up implementation tickets/tests, enforcement could remain aspirational.
- The phrase "local secret store or equivalent operator-controlled local custody" is intentionally broad and may need tightening during implementation to avoid inconsistent single-Device storage choices.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260917T115652Z-UBU-Q0136/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260917T115652Z-UBU-Q0136/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260917T115652Z-UBU-Q0136"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
