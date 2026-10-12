# Model-Committee Review

Run: `20261012T150721Z-UBU-Q0174`  
Question: `UBU-Q0174`  
Base commit: `49aae53f500a7f3c0e3c006683e0751947cab0da`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0174-openai-gpt-6-20261012`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0174-openai-gpt-6-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0174-openai-gpt-6-20261012` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0174-openai-gpt-6-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0174-openai-gpt-6-20261012` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0174-openai-gpt-6-20261012` | `openai` | `codex` | yes | 96 | Mechanical validation passes, and changes are confined to the three allowed files. The proposal directly specifies domain limits, missing-context and hazardous-uncertainty stops, contradictory observations, professional and emergency escalation, visible cancellation, and fresh resumption. It correctly uses Status: Solved, preserves existing privacy, attention, authority and evidence contracts, and leaves expert packets and later Technique execution scoped to their existing work. Some cancellation rules repeat existing contracts, but no material scope expansion or blocking defect was found. | None | Workflow-specific release envelopes and professional-qualification checks require implementation and validation before enablement; these process requirements do not establish operational safety. |
| `ubu-q0174-openai-gpt-6-20261012` | `openai` | `claude` | yes | 82 | Mechanical validation passes: the patch applies, only the three allowlisted files change, and the question block uses Status: Solved with Resolved by: UBU-D0330 and an updated Resolution while leaving other metadata untouched. The content is directly responsive to every element of the Current direction: domain limits (release envelope and excluded domains), missing context, hazardous uncertainty, contradictory observations and failed verification, user-visible stop behavior, 'camera access is not domain competence', and explicit bounding that guidance is not professional replacement. It also handles resumption, separates escalation from contact/export/action authority, defers expert packets to UBU-Q0175, and reuses existing session/evidence/Log contracts rather than adding kernel objects, which keeps scope reasonable for a Process decision. The main shortcomings are the §17.12 placement mismatch, the unspecified release-envelope artifact, and heavy density, none of which block adoption. Score reflects a valid, auditable, complete resolution with modest follow-up needed. | Add a one-line pointer in or near §17.12 (or state in the Resolution why §21.1 is the correct home) so the 'Delta §17.12' direction in the question resolves to §21.1.6. | The question's Current direction points at 'Delta §17.12', but the proposal adds the contract as DESIGN.md §21.1.6 and does not touch or cross-reference §17.12; readers following the question metadata may not find the new section., Introduces a 'release envelope' as a new controller-owned versioned artifact (supported workflows/steps, prerequisites, clarification limits, professional roles). It is declared without a schema, owner or lifecycle, so it may become a de facto new object even though the text says no new kernel field, provenance kind or Log event is selected., The section is very dense and leans on many cross-references (§§21.1.2–21.1.5, §17.0, §17.10, §22.4, UBU-D0324/D0325/D0329, UBU-Q0166/Q0168/Q0175). Any of those that do not say what the proposal assumes would leave dangling obligations; this could not be verified from the diff alone., Prescriptive UI/behavior details (exact 'Guidance stopped' notice contents, emergency-escalation message rules, restricted-attention output envelope) go slightly beyond a process contract and may need to be reconciled with existing session-lifecycle and attention sections., The urgent-harm row forbids 'waiting for more images or diagnostic questioning'; combined with the fallibility caveat this is sound, but it depends on an unspecified classifier of 'possible immediate serious harm', which is itself hazardous uncertainty the proposal does not bound. |

## Selected Summary

Define bounded realtime guidance, safety stops, professional escalation and fresh resumption; resolve UBU-Q0174 through UBU-D0330.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Introduces a 'release envelope' as a new controller-owned versioned artifact (supported workflows/steps, prerequisites, clarification limits, professional roles). It is declared without a schema, owner or lifecycle, so it may become a de facto new object even though the text says no new kernel field, provenance kind or Log event is selected.
- Prescriptive UI/behavior details (exact 'Guidance stopped' notice contents, emergency-escalation message rules, restricted-attention output envelope) go slightly beyond a process contract and may need to be reconciled with existing session-lifecycle and attention sections.
- The question's Current direction points at 'Delta §17.12', but the proposal adds the contract as DESIGN.md §21.1.6 and does not touch or cross-reference §17.12; readers following the question metadata may not find the new section.
- The section is very dense and leans on many cross-references (§§21.1.2–21.1.5, §17.0, §17.10, §22.4, UBU-D0324/D0325/D0329, UBU-Q0166/Q0168/Q0175). Any of those that do not say what the proposal assumes would leave dangling obligations; this could not be verified from the diff alone.
- The urgent-harm row forbids 'waiting for more images or diagnostic questioning'; combined with the fallibility caveat this is sound, but it depends on an unspecified classifier of 'possible immediate serious harm', which is itself hazardous uncertainty the proposal does not bound.
- Workflow-specific release envelopes and professional-qualification checks require implementation and validation before enablement; these process requirements do not establish operational safety.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T150721Z-UBU-Q0174/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T150721Z-UBU-Q0174/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T150721Z-UBU-Q0174"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
