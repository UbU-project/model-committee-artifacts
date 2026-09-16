# Model-Committee Review

Run: `20260916T192536Z-UBU-Q0150`  
Question: `UBU-Q0150`  
Base commit: `cc9181c72201abb1e0e28c0086bd3f8529975f80`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0150-openai-gpt5-codex-20260916-001`

## Disagreement Flags

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
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0150-openai-gpt5-codex-20260916-001` |
| `work-score` | `claude` | `sonnet` | `ubu-q0150-openai-gpt5-codex-20260916-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0150-openai-gpt5-codex-20260916-001` |
| `work-score` | `claude` | `sonnet` | `ubu-q0150-openai-gpt5-codex-20260916-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0150-openai-gpt5-codex-20260916-001` | `openai` | `codex` | yes | 94 | The patch is valid, minimal, and directly answers UBU-Q0150 by defining stability scopes for redacted object refs and compartment grouping refs while stating the acceptable correlation boundary. It preserves the open-question schema, resolves the question with a linked decision, and modifies only the expected documentation files. The added decision is auditable and aligned with the privacy constraints implied by the question. | None | The decision is intentionally strict: rotating object handles on every source-object version change may reduce continuity for restricted-device UI flows that want stable identity across edited versions., The optional compartment_ref rule depends on future policy language to define when grouping leakage is acceptable. |
| `ubu-q0150-openai-gpt5-codex-20260916-001` | `openai` | `claude` | yes | 88 | Mechanical validation confirms the patch applies cleanly and touches only DECISIONS.md and OPEN_QUESTIONS.md, both plausible allowlisted files for a decision-resolving change. The new UBU-D0253 decision directly answers the question's two sub-parts (handle stability scope and acceptable correlation risk) with a specific, falsifiable policy: object handles stable per Device+version+window with enumerated rotation triggers, Compartment handles omitted by default and stable only per Device+window+policy-epoch when explicitly permitted. The OPEN_QUESTIONS.md edit follows the existing single-line metadata convention and updates Status/Resolved by/Resolution fields consistently, preserving the question schema. Scope is appropriately narrow — no unrelated files touched, no extraneous decisions added. The only proposal in this set, and it is well-formed and directly responsive to the question. | None | The rule that handles rotate on 'target Device authority' change but are otherwise stable within one projection window could still allow moderate correlation if projection windows are long-lived; the decision doesn't bound window length., Omission-by-default for compartment_ref could break existing UI grouping features that assumed some compartment_ref was always present, requiring downstream implementation changes not covered here., New decision UBU-D0253 doesn't cross-reference or reconcile with any existing decisions about redaction/projection windows in DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md, so consistency with §10/§12 wording is asserted but not verified against file content. |

## Selected Summary

Resolve UBU-Q0150 by defining redacted handles as local rotating aliases: object handles are stable only for one receiving Device, one redacted source-object version, and one projection window; Compartment grouping handles are omitted by default and, when allowed, are stable only for one Device/window/policy epoch.

## Changed Files

- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- New decision UBU-D0253 doesn't cross-reference or reconcile with any existing decisions about redaction/projection windows in DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md, so consistency with §10/§12 wording is asserted but not verified against file content.
- Omission-by-default for compartment_ref could break existing UI grouping features that assumed some compartment_ref was always present, requiring downstream implementation changes not covered here.
- The decision is intentionally strict: rotating object handles on every source-object version change may reduce continuity for restricted-device UI flows that want stable identity across edited versions.
- The optional compartment_ref rule depends on future policy language to define when grouping leakage is acceptable.
- The rule that handles rotate on 'target Device authority' change but are otherwise stable within one projection window could still allow moderate correlation if projection windows are long-lived; the decision doesn't bound window length.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260916T192536Z-UBU-Q0150/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260916T192536Z-UBU-Q0150/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260916T192536Z-UBU-Q0150"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
