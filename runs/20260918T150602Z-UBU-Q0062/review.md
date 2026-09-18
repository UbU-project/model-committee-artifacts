# Model-Committee Review

Run: `20260918T150602Z-UBU-Q0062`  
Question: `UBU-Q0062`  
Base commit: `22733320988423b6ba7273753aaf472b147dce68`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0062-openai-gpt-5-codex-20260918`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0062-openai-gpt-5-codex-20260918` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0062-openai-gpt-5-codex-20260918` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0062-openai-gpt-5-codex-20260918` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0062-openai-gpt-5-codex-20260918` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0062-openai-gpt-5-codex-20260918` | `openai` | `codex` | yes | 92 | The patch cleanly resolves UBU-Q0062 with an auditable decision, updates the design text in the relevant Association section, and preserves the open-question schema while changing only allowed files. It directly covers required claim types, source-kind distinctions, generated-attestation provenance, review and dispute states, supersession semantics, source-specific trust guidance, and privacy-preserving disclosure policy. The scope is appropriately narrow and consistent with the existing Q0061 Association framing. Remaining concerns are refinements rather than blockers. | None | Trust/confidence guidance remains qualitative rather than defining a concrete weighting model or per-source scoring rubric., DECISIONS.md disclosure paragraph omits reputation from the explicit non-leak list even though DESIGN.md and the resolution include it., Dispute actions by an affected Association may need later clarification around who has standing or authority to act for a non-organization Association. |
| `UBU-Q0062-openai-gpt-5-codex-20260918` | `openai` | `claude` | yes | 88 | The patch applies cleanly and touches only the allowlisted files (DESIGN.md, DECISIONS.md, OPEN_QUESTIONS.md). It directly answers all six subquestions: claim type taxonomy, source-kind distinctions (user_authored/imported/worker_generated_candidate/llm_generated_candidate), required provenance fields for generated attestations, dispute/annotate/accept/reject/supersede semantics via review_status, trust/confidence separation across source types, and disclosure policy requirements to prevent leakage. It follows the established pattern from UBU-D0268 (Associations), keeps attestations as append-only/immutable with supersession rather than mutation, and explicitly avoids granting authority or organization-mode legitimacy from attestations alone, respecting prior boundaries in the codebase. OPEN_QUESTIONS.md preserves the single-line metadata format and updates status/resolution correctly. Scope is proportionate to the question; no unrelated files or speculative features are introduced. | None | Introduces many new enumerated fields and states (claim types, source kinds, review statuses) in a single decision, which increases the surface area a future decision must stay consistent with, Confidence/trust distinction is asserted but not tied to any concrete scoring mechanism, leaving implementation details for a later question |

## Selected Summary

Resolve UBU-Q0062 by defining AssociationAttestations as immutable, evidence-backed scoped claims with explicit source kind, provenance, confidence, review/dispute states, supersession semantics, and privacy-preserving disclosure rules.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Confidence/trust distinction is asserted but not tied to any concrete scoring mechanism, leaving implementation details for a later question
- DECISIONS.md disclosure paragraph omits reputation from the explicit non-leak list even though DESIGN.md and the resolution include it.
- Dispute actions by an affected Association may need later clarification around who has standing or authority to act for a non-organization Association.
- Introduces many new enumerated fields and states (claim types, source kinds, review statuses) in a single decision, which increases the surface area a future decision must stay consistent with
- Trust/confidence guidance remains qualitative rather than defining a concrete weighting model or per-source scoring rubric.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260918T150602Z-UBU-Q0062/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260918T150602Z-UBU-Q0062/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260918T150602Z-UBU-Q0062"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
