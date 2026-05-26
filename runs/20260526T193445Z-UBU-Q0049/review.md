# Model-Committee Review

Run: `20260526T193445Z-UBU-Q0049`  
Question: `UBU-Q0049`  
Base commit: `2c410f5b6f469b90fbc850528e479eea059d7fab`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0049-20260526-001`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 83.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0049-20260526-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0049-20260526-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0049-20260526-001` | `codex` | `claude` | yes | 83 | The proposal comprehensively resolves all eight subquestions of UBU-Q0049. It defines a minimal, structured package artifact model (manifest.json, claim_register.json, evidence_index.json, media_refs.json, export_review.json, approvals.json, publication_plan.json), a four-status claim-support taxonomy, explicit media provenance requirements, append-only approval gates, hard Compartment/Identity upper bounds, a clear Phase 1 versus post-MVP scope boundary, audience-specific Objective configuration, and the Automation Worker versus external publication system boundary. All eight subquestions are addressed directly and with sufficient specificity to unblock Phase 1 implementation. The stale DECISIONS.md reference listing UBU-Q0049 as open is correctly removed, and the tombstone format matches the established pattern for resolved questions. Scope is appropriate: only DECISIONS.md, DESIGN.md, and OPEN_QUESTIONS.md are modified, and the DESIGN.md edits align with the sections the decision references (§§2.6, 4.1.3). Deductions: the patch required --recount normalization indicating a generation-time issue (-4); the DESIGN.md rewrites are slightly more extensive than strictly necessary given the decision document stands on its own (-3); a minor tombstone formatting gap and unverifiable decision number sequencing (-4) combined (-6 total from these concerns). Overall a sound and directly responsive resolution. | Add blank line between 'Resolved. See UBU-D0180.' and the trailing '---' separator in OPEN_QUESTIONS.md to match the formatting of other tombstones in the file., Verify that UBU-D0180 is the correct next sequential decision number at the base commit before merging. | Patch header was corrupt and required --recount normalization; indicates patch was not cleanly generated at the stated base commit., UBU-D0180 decision number may conflict if another decision was added between the base commit and the authoring point; cannot verify sequential integrity without reading the full file., DESIGN.md §2.6 and §4.1.3 rewrites are substantial and condense some original language about generalization to project types; subtle meaning loss is possible., Tombstone in OPEN_QUESTIONS.md drops all subquestion text, making historical audit harder if DECISIONS.md is ever unavailable, though this appears to match the established pattern for other resolved questions., Missing blank line before the `---` separator in the OPEN_QUESTIONS.md tombstone block (minor formatting inconsistency). |

## Selected Summary

Resolve UBU-Q0049 by defining the Release Outreach Pipeline package model, claim labels, media provenance, approval gates, privacy/export boundary, Phase 1 scope, and Automation Worker versus external publication boundary.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- DESIGN.md §2.6 and §4.1.3 rewrites are substantial and condense some original language about generalization to project types; subtle meaning loss is possible.
- Missing blank line before the `---` separator in the OPEN_QUESTIONS.md tombstone block (minor formatting inconsistency).
- Patch header was corrupt and required --recount normalization; indicates patch was not cleanly generated at the stated base commit.
- Tombstone in OPEN_QUESTIONS.md drops all subquestion text, making historical audit harder if DECISIONS.md is ever unavailable, though this appears to match the established pattern for other resolved questions.
- UBU-D0180 decision number may conflict if another decision was added between the base commit and the authoring point; cannot verify sequential integrity without reading the full file.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T193445Z-UBU-Q0049/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T193445Z-UBU-Q0049/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T193445Z-UBU-Q0049"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
