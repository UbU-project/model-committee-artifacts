# Model-Committee Review

Run: `20261012T131052Z-UBU-Q0166`  
Question: `UBU-Q0166`  
Base commit: `f83c76714e4ab44f0e6c87e405057702d5bf4613`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0166-openai-gpt-6-20261012-carrier-evidence`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 77.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-openai-gpt-6-20261012-carrier-evidence` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-openai-gpt-6-20261012-carrier-evidence` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-openai-gpt-6-20261012-carrier-evidence` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-openai-gpt-6-20261012-carrier-evidence` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0166-openai-gpt-6-20261012-carrier-evidence` | `openai` | `codex` | yes | 78 | Mechanical validation passes, changes stay within allowed files, and UBU-Q0166 retains valid Open metadata. Local source inspection corroborates the pinned carrier types, payload reconstruction and review-presentation limitations. The proposal carefully distinguishes generic representation from adapter compatibility and failed field choices from schema gaps. It provides useful, conservative partial evidence, but does not complete the selected host-mapping and fidelity work. | Complete and document the revision-pinned mappings and UBU-D0321 fidelity fixtures across the required host paths, or demonstrate a governing host-contract gap and route it to separate versioned review., Provide reproducible probe commands or a pinned harness and exact serialized outputs supporting the reported runtime comparisons. | The synthetic specimen tests candidate claim links, not preservation of the required claim content, qualifications, attribution, confidence semantics and source history., Evidence resolution, adapter rejection, normalization, persistence/reload, review, admission and authorized exchange remain untested., Runtime results lack exact invocation commands or a pinned probe harness and retrieved-output artifacts. |
| `UBU-Q0166-openai-gpt-6-20261012-carrier-evidence` | `openai` | `claude` | yes | 76 | Patch applies cleanly, touches only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md, and leaves the UBU-Q0166 metadata line and neighboring UBU-Q0167 block untouched with Status: Open. It is directly responsive to both halves of the question: it identifies a concrete pinned candidate mapping (inline payload.value.perceptual_qualifiers survives schema validation and serde round trip; string-valued field_provenance rejects structured content) and states a decision rule for when a failed mapping counts as a schema gap versus a field-choice or adapter defect. It is honest about scope, repeatedly disclaiming a passing adapter or schema gap, labelling source-inspection rows as such, and noting that generic layers accepted an unknown profile so adapter checks remain required. The unknown-profile acceptance finding is genuinely useful. Weaknesses are that the evidence is partial and unverifiable from the patch, the specimen is synthetic, exchange paths are untouched, and the Current direction paragraph is extended by appending rather than restructuring. No fixes are strictly required; a follow-up should commit the probe fixtures or a rerunnable script so the recorded outcomes can be independently reproduced. | None | The executed-probe claims (tools/validate-fixtures runs, AdvisoryCandidate serde round trips, rustc/serde/jsonschema versions, five pinned host commits) cannot be verified from the patch; the design doc will carry them as recorded evidence on the strength of the author's validation notes alone., Only two of the five host paths (candidate schema, candidate Rust/JSON) have actual execution results; store/reload, normalization/review and review presentation are source inspection, and authorized exchange is not probed at all, so the question's core ask remains mostly unanswered., The carrier specimen uses an unresolved synthetic evidence_ref, so it demonstrates link-carrier shape preservation only, not UBU-D0321 claim/qualifier fidelity with resolved evidence revisions., UBU-D0323 is written with Status: Accepted although the question is human-approval-required; this depends on repository convention that acceptance is gated at proposal application rather than in the decision text., The DESIGN.md §21.1.1 insert embeds implementation-specific commit hashes, toolchain versions and Rust type names into the design contract; this is what revision pinning requires, but it will go stale and must not be read as a dependency pin., The decision number UBU-D0323 assumes the prior entry is UBU-D0322; the diff context does not show that number. |

## Selected Summary

Record pinned candidate carrier results and distinguish mapping failures from schema gaps, keeping UBU-Q0166 open for complete host evidence.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Evidence resolution, adapter rejection, normalization, persistence/reload, review, admission and authorized exchange remain untested.
- Only two of the five host paths (candidate schema, candidate Rust/JSON) have actual execution results; store/reload, normalization/review and review presentation are source inspection, and authorized exchange is not probed at all, so the question's core ask remains mostly unanswered.
- Runtime results lack exact invocation commands or a pinned probe harness and retrieved-output artifacts.
- The DESIGN.md §21.1.1 insert embeds implementation-specific commit hashes, toolchain versions and Rust type names into the design contract; this is what revision pinning requires, but it will go stale and must not be read as a dependency pin.
- The carrier specimen uses an unresolved synthetic evidence_ref, so it demonstrates link-carrier shape preservation only, not UBU-D0321 claim/qualifier fidelity with resolved evidence revisions.
- The decision number UBU-D0323 assumes the prior entry is UBU-D0322; the diff context does not show that number.
- The executed-probe claims (tools/validate-fixtures runs, AdvisoryCandidate serde round trips, rustc/serde/jsonschema versions, five pinned host commits) cannot be verified from the patch; the design doc will carry them as recorded evidence on the strength of the author's validation notes alone.
- The synthetic specimen tests candidate claim links, not preservation of the required claim content, qualifications, attribution, confidence semantics and source history.
- UBU-D0323 is written with Status: Accepted although the question is human-approval-required; this depends on repository convention that acceptance is gated at proposal application rather than in the decision text.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T131052Z-UBU-Q0166/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T131052Z-UBU-Q0166/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T131052Z-UBU-Q0166"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
