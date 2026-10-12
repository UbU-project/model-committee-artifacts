# Model-Committee Review

Run: `20261012T140548Z-UBU-Q0169`  
Question: `UBU-Q0169`  
Base commit: `9f312041296214e0e279ebfd61b4ae5a5ab1342d`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0169-openai-gpt-6-local-media-1`

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
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0169-openai-gpt-6-local-media-1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0169-openai-gpt-6-local-media-1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0169-openai-gpt-6-local-media-1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0169-openai-gpt-6-local-media-1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0169-openai-gpt-6-local-media-1` | `openai` | `codex` | yes | 96 | Mechanical validation confirms clean application, allowed files and question-schema preservation, including Status: Solved. The architecture directly covers segmentation, buffering, timestamps, source boundaries, scoped operations, cancellation and intermediate retention. Cross-references remain consistent, and numerical acceptance, retention defaults, operator controls and implementation evidence retain their existing scope. No blocking defect is evident. | None | Adapter enablement still requires measured finite limits, an effective retention policy, and demonstrated isolation, revocation and artifact cleanup. |
| `UBU-Q0169-openai-gpt-6-local-media-1` | `openai` | `claude` | yes | 87 | Mechanical validation passed: patch applies, allowlist passed, question schema passed. The proposal directly answers UBU-Q0169's question (segmentation, buffering, timestamps, cancellation, source boundaries, access grants, intermediate-artifact retention, and three scoped operations for frame extraction, event detection and section isolation). It records UBU-D0326 as the next decision number after UBU-D0325, inserts §21.1.5 immediately after §21.1.4 and before §21.2, and marks the question `Status: Solved` with `Resolved by: UBU-D0326` while preserving all other metadata fields and other question blocks. The two edits to existing DESIGN.md paragraphs are limited to updating cross-references that previously said segmentation remained open in UBU-Q0169, so they are necessary consistency changes rather than scope creep. The text consistently keeps scene/OCR/transcript output as untrusted data, keeps results as Discovery/advisory evidence, and defers numeric acceptance, retention defaults and operator controls to UBU-Q0168, UBU-Q0170 and UBU-Q0171, matching the question's stated direction and the repo's recurring 'policy recorded, not enforcement shipped' hedge. Deductions are for unverifiable section cross-references, the quasi-schema operation names, and the density of the added contract. | Verify that DESIGN.md §12.2, §21.1.3 and §§21.3–21.4 exist at the base commit and point at capture controls and export/routing content as the new text claims; adjust references if not. | New DESIGN.md §21.1.5 cross-references §12.2, §21.1.3 and §§21.3–21.4; these section numbers are not visible in the patch context and should be confirmed to exist so the contract does not dangle., The section introduces named logical operations (`media.frames.extract`, `media.events.detect`, `media.sections.isolate`) and an adapter profile label (`local-media/1`). The text hedges that this is a boundary, not a wire schema, but these names may be read later as frozen interface commitments., The section is long and dense (~40 lines of tightly packed constraints). It matches the style of adjacent §21.1.x content but will be hard to audit line by line and may overlap with retention rules still owned by UBU-Q0170/UBU-Q0171., The statement 'Continuous capture cannot be enabled with unresolved effective retention policy' adds a hard gate coupling this decision to UBU-Q0170, which is reasonable but is new policy rather than pure architecture. |

## Selected Summary

Define bounded local AV segmentation and purpose-scoped frame extraction, event detection and section isolation.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Adapter enablement still requires measured finite limits, an effective retention policy, and demonstrated isolation, revocation and artifact cleanup.
- New DESIGN.md §21.1.5 cross-references §12.2, §21.1.3 and §§21.3–21.4; these section numbers are not visible in the patch context and should be confirmed to exist so the contract does not dangle.
- The section introduces named logical operations (`media.frames.extract`, `media.events.detect`, `media.sections.isolate`) and an adapter profile label (`local-media/1`). The text hedges that this is a boundary, not a wire schema, but these names may be read later as frozen interface commitments.
- The section is long and dense (~40 lines of tightly packed constraints). It matches the style of adjacent §21.1.x content but will be hard to audit line by line and may overlap with retention rules still owned by UBU-Q0170/UBU-Q0171.
- The statement 'Continuous capture cannot be enabled with unresolved effective retention policy' adds a hard gate coupling this decision to UBU-Q0170, which is reasonable but is new policy rather than pure architecture.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T140548Z-UBU-Q0169/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T140548Z-UBU-Q0169/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T140548Z-UBU-Q0169"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
