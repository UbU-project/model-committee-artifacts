# Model-Committee Review

Run: `20261011T200833Z-UBU-Q0128`  
Question: `UBU-Q0128`  
Base commit: `ab0668ab7395276fc6ecfae4aaf86312160a23e4`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0128-openai-gpt-6-20261011`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 86.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0128-openai-gpt-6-20261011` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0128-openai-gpt-6-20261011` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0128-openai-gpt-6-20261011` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0128-openai-gpt-6-20261011` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0128-openai-gpt-6-20261011` | `openai` | `codex` | yes | 89 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch directly addresses the initial permission envelope, controller authority, scoped context, interruption handling, audit and fresh review while preserving existing MVP commitments. It appropriately keeps UBU-Q0128 Open and distinguishes candidate capture from admitted state. Coverage is intentionally partial: detailed mode profiles, messaging behavior, schemas and confirmation grammar remain unresolved. | None | Numeric limits and exact budget, output and confirmation schemas remain undefined; implementation requires further policy decisions., Activity uncertainty, interruption and device-specific behavior require validation before enabling restricted-attention use. |
| `UBU-Q0128-openai-gpt-6-20261011` | `openai` | `claude` | yes | 84 | The patch applies cleanly, touches only the three allowlisted files, and keeps UBU-Q0128 at Status: Open with its metadata line, Phase 2 tag and scoring fields unchanged. It is directly responsive: it adds one new decision (UBU-D0309), one DESIGN.md subsection, and a matching Current direction / Resolution update, covering subquestions 1–4, 7–9, 11–12, 16–19 and 20 while explicitly leaving quantitative profiles, exact budget/LLM-output schemas, confirmation grammar, richer messaging/media/coaching integrations and naming open. The content is consistent with the already-recorded constraints from UBU-D0292/UBU-D0293: models stay advisory, affect stays user-declared, no spoken confirmation authorizes consequential external actions, and UBU-Q0183 retains the delivery sequence. It is auditable and avoids granting new authority. Deductions are for unverifiable cross-references, the rewrite of an existing direction paragraph, and the density of the new design text. | Before applying, confirm that §21.1.1 exists in DESIGN.md (so §21.1.2 is correctly numbered) and that §21.3, §12.2, UBU-Q0129 and UBU-Q0170–UBU-Q0171 refer to what the new text claims; adjust references if any are wrong., Confirm with the operator that replacing the 'affect model is central' paragraph with the user-declared framing is the intended change to recorded direction, or restore the original paragraph and add the new sentence alongside it. | Several cross-references cannot be verified from the prompt alone: UBU-Q0129 as the Original Narration question, UBU-Q0170–UBU-Q0171 as raw-media retention, DESIGN.md §21.3 as ContextBundle, §12.2 as raw-AV locality, and the existence of a §21.1.1 that makes the new §21.1.2 heading number correct., The patch rewrites the existing 'The affect model is central' paragraph in Current direction rather than only appending. The rewrite is consistent with UBU-D0292's 'affect remains user-declared', but it removes prior recorded direction text and should be reviewed as a deliberate change., The DESIGN.md addition is dense (roughly 1,000 words in one subsection) and front-loads many normative rules; later schema and profile work will need to amend it carefully to avoid contradiction., The decision labels itself Accepted while the question is 'Human approval required'; the proposal correctly sets requires_human_review, but the Accepted status should not be published without operator sign-off., 'Resolved by: None' is retained even though UBU-D0309 partially answers the question; this matches how UBU-D0292/UBU-D0293 were treated, but the convention should be confirmed. |

## Selected Summary

Adopt a controller-enforced restricted-attention capture envelope, with session-local InteractionBudget, high-risk deferral and fresh review after interruption or context changes.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- 'Resolved by: None' is retained even though UBU-D0309 partially answers the question; this matches how UBU-D0292/UBU-D0293 were treated, but the convention should be confirmed.
- Activity uncertainty, interruption and device-specific behavior require validation before enabling restricted-attention use.
- Numeric limits and exact budget, output and confirmation schemas remain undefined; implementation requires further policy decisions.
- Several cross-references cannot be verified from the prompt alone: UBU-Q0129 as the Original Narration question, UBU-Q0170–UBU-Q0171 as raw-media retention, DESIGN.md §21.3 as ContextBundle, §12.2 as raw-AV locality, and the existence of a §21.1.1 that makes the new §21.1.2 heading number correct.
- The DESIGN.md addition is dense (roughly 1,000 words in one subsection) and front-loads many normative rules; later schema and profile work will need to amend it carefully to avoid contradiction.
- The decision labels itself Accepted while the question is 'Human approval required'; the proposal correctly sets requires_human_review, but the Accepted status should not be published without operator sign-off.
- The patch rewrites the existing 'The affect model is central' paragraph in Current direction rather than only appending. The rewrite is consistent with UBU-D0292's 'affect remains user-declared', but it removes prior recorded direction text and should be reviewed as a deliberate change.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261011T200833Z-UBU-Q0128/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261011T200833Z-UBU-Q0128/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261011T200833Z-UBU-Q0128"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
