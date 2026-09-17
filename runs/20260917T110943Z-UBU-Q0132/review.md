# Model-Committee Review

Run: `20260917T110943Z-UBU-Q0132`  
Question: `UBU-Q0132`  
Base commit: `87c6cc6c70c9794193dc4d016e5fe197fd5fc7b3`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-5-codex-UBU-Q0132-001`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0132-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0132-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0132-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0132-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-5-codex-UBU-Q0132-001` | `openai` | `codex` | yes | 92 | The proposal applies cleanly, touches only the expected documentation files, preserves the open-question schema while marking UBU-Q0132 resolved, and adds a focused decision plus DESIGN summary. It directly answers all subquestions: tombstone shape, decomposition parent retirement via tombstone plus snapshot, undo without resurrecting purged payload, retained versus purged fields, projection deletion as separate idempotent projection state, and envelope metadata via UBU-Q0130. The remaining risks are implementation-detail ambiguities rather than blockers for this design decision. | None | The decision intentionally remains documentation-level and does not define concrete storage schema, retention windows, or migration behavior for tombstone records., The phrase "policy-safe Compartment refs" leaves some discretion that must be aligned carefully with UBU-Q0144 and UBU-Q0130 to avoid leaking restricted compartment identity or undermining local enforcement., Undo semantics depend on the lifecycle of the decomposition parent snapshot; implementations will need clear rules for when that snapshot is restorable versus purged or redacted beyond use. |
| `openai-gpt-5-codex-UBU-Q0132-001` | `openai` | `claude` | yes | 85 | The patch applies cleanly per mechanical validation and touches only OPEN_QUESTIONS.md, DECISIONS.md, and DESIGN.md, all appropriate for resolving an open question with a new decision. It substantively answers all six subquestions: tombstone record shape (subquestion 1), decomposition parent retirement as tombstone+snapshot (2), undo semantics that refuse to resurrect purged payload (3), explicit retain-vs-purge field list (4), projection-deletion as separate idempotent projection_state obligations (5), and envelope metadata via the UBU-D0258 mutation path (6). It preserves the question metadata schema (Status, Priority, Answerability, Resolved by, etc.) and correctly marks the question Solved while leaving UBU-Q0144 (purge propagation) explicitly still open, matching the 'Current direction' note in the source question. The new UBU-D0259 decision is well-integrated with DECISIONS.md and DESIGN.md, and the scope is minimal and auditable — no unrelated files or speculative additions. The main uncertainty is whether cross-references to UBU-D0258 and other decisions are numbered consistently with the actual current repo state, which I cannot fully verify from the diff alone, but the structural content and reasoning are sound and directly responsive. | None | References UBU-D0258 mutation-envelope path and DESIGN.md §23.1 numbering without this context confirming those exist and are numbered consistently in the live repo; if stale, the decision text could misalign with actual document structure., The tombstone record shape is fairly detailed (many named fields) which increases the surface area that later decisions must stay consistent with; if UBU-Q0144 (purge propagation across offline Devices) resolves differently, some retained-field choices here may need revision., Undo-refusal-on-purge behavior ('create a repair or clarification path') is stated as an option rather than a firm requirement, leaving a soft edge that a later decision could tighten or contradict. |

## Selected Summary

Resolve Phase 1b canonical deletion as tombstone-first lifecycle state with best-effort payload purge, preserving only structural identity, causality, audit, undo eligibility, and projection-cleanup refs.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DECISIONS.md`
- `DESIGN.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- References UBU-D0258 mutation-envelope path and DESIGN.md §23.1 numbering without this context confirming those exist and are numbered consistently in the live repo; if stale, the decision text could misalign with actual document structure.
- The decision intentionally remains documentation-level and does not define concrete storage schema, retention windows, or migration behavior for tombstone records.
- The phrase "policy-safe Compartment refs" leaves some discretion that must be aligned carefully with UBU-Q0144 and UBU-Q0130 to avoid leaking restricted compartment identity or undermining local enforcement.
- The tombstone record shape is fairly detailed (many named fields) which increases the surface area that later decisions must stay consistent with; if UBU-Q0144 (purge propagation across offline Devices) resolves differently, some retained-field choices here may need revision.
- Undo semantics depend on the lifecycle of the decomposition parent snapshot; implementations will need clear rules for when that snapshot is restorable versus purged or redacted beyond use.
- Undo-refusal-on-purge behavior ('create a repair or clarification path') is stated as an option rather than a firm requirement, leaving a soft edge that a later decision could tighten or contradict.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260917T110943Z-UBU-Q0132/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260917T110943Z-UBU-Q0132/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260917T110943Z-UBU-Q0132"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
