# Model-Committee Review

Run: `20260918T144535Z-UBU-Q0061`  
Question: `UBU-Q0061`  
Base commit: `112238fb10a75ba0c9d568c59bda163dd736bf2e`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0061-gpt5-codex-association-model-v1`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0061-gpt5-codex-association-model-v1` |
| `work-score` | `claude` | `sonnet` | `ubu-q0061-gpt5-codex-association-model-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0061-gpt5-codex-association-model-v1` |
| `work-score` | `claude` | `sonnet` | `ubu-q0061-gpt5-codex-association-model-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0061-gpt5-codex-association-model-v1` | `openai` | `codex` | yes | 91 | The patch is valid, narrow, and directly resolves UBU-Q0061. It clearly establishes Association as first-class, Identity-scoped, perspective-bound, and capable of existing privately in user_mode; distinguishes it from Organization Identity, Relationship, and External Reference; covers informal groups, lifecycle events, organization_mode promotion criteria, disclosure policy, provenance, confidence, and Phase 3 versus later-research boundaries. It modifies only the expected design/question/decision files and preserves the open-question structure while marking the question resolved. Remaining concerns are mostly follow-up precision around event objects and existing Group terminology, not blockers. | None | Introduces AssociationAttestation and lifecycle event terminology without defining a concrete event schema, which may need a follow-up data-model decision before implementation., Adds Association alongside existing Group, GroupMembership, and RoleAssignment language, creating a small risk of terminology overlap unless later docs clarify whether Group is superseded, aliased, or lower-level. |
| `ubu-q0061-gpt5-codex-association-model-v1` | `openai` | `claude` | yes | 83 | The proposal directly resolves UBU-Q0061 by defining a first-class Association object, addressing all seven subquestions: it distinguishes Association from Organization Identity/Relationship/External Reference, confirms it can live entirely in user_mode, lists minimum fields, covers informal groups, sets a clear organization_mode promotion threshold, defines lifecycle events as append-only records, and scopes Phase 3 requirements versus deferred research. It correctly flips OPEN_QUESTIONS.md status to Solved/Resolved with a new UBU-D0268 decision entry, consistent with the existing schema and cross-references to UBU-D0265/0266/0267. Changes are confined to the three expected files, mechanical validation confirms the patch applies cleanly and only touches the allowlisted files, and it explicitly defers cross-user reconciliation, dispute semantics, and Skill Barter governance to later questions, avoiding scope creep. As a single proposal (not a decomposition), decomposition_quality is not_applicable. | None | Introduces references to GroupMembership, RoleAssignment, and AssociationAttestation record types without fully defining their schemas, creating potential dangling concepts for future questions to reconcile., DESIGN.md addition assumes an existing §1 numbering context (1.2) that isn't shown in the diff; if the surrounding section doesn't match the intended narrative flow, it could read as a non sequitur., Broad set of minimum fields for the Association record may be over-specified for a 'Post-MVP' data model decision, slightly increasing future migration risk if fields need revision. |

## Selected Summary

Resolve UBU-Q0061 by introducing a first-class Association as an Identity-scoped, perspective-bound coordination structure, with minimum fields, lifecycle semantics, informal-group handling, and a clear threshold for promotion to organization_mode.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DECISIONS.md`
- `DESIGN.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Adds Association alongside existing Group, GroupMembership, and RoleAssignment language, creating a small risk of terminology overlap unless later docs clarify whether Group is superseded, aliased, or lower-level.
- Broad set of minimum fields for the Association record may be over-specified for a 'Post-MVP' data model decision, slightly increasing future migration risk if fields need revision.
- DESIGN.md addition assumes an existing §1 numbering context (1.2) that isn't shown in the diff; if the surrounding section doesn't match the intended narrative flow, it could read as a non sequitur.
- Introduces AssociationAttestation and lifecycle event terminology without defining a concrete event schema, which may need a follow-up data-model decision before implementation.
- Introduces references to GroupMembership, RoleAssignment, and AssociationAttestation record types without fully defining their schemas, creating potential dangling concepts for future questions to reconcile.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260918T144535Z-UBU-Q0061/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260918T144535Z-UBU-Q0061/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260918T144535Z-UBU-Q0061"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
