# Model-Committee Review

Run: `20261012T122730Z-UBU-Q0128`  
Question: `UBU-Q0128`  
Base commit: `47c766e5ffea71c99f177a80f55df1488d026659`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0128-openai-playback-referents-v1`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0128-openai-playback-referents-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0128-openai-playback-referents-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0128-openai-playback-referents-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0128-openai-playback-referents-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0128-openai-playback-referents-v1` | `openai` | `codex` | yes | 94 | Supplied mechanical validation confirms clean application, allowed files and preserved question schema. The patch directly addresses initial-envelope playback referents and vague commands while preserving UBU-D0309–UBU-D0311 authority boundaries. It adds explicit ambiguity handling, interruption invalidation and replay protections without expanding actions or claiming full resolution. UBU-Q0128 appropriately remains Open. | None | Correct dispatch depends on adapters enforcing target and control-revision bindings; adapter support is not demonstrated., Acceptance fixtures remain unexecuted, so this establishes a design contract rather than validated runtime behavior. |
| `UBU-Q0128-openai-playback-referents-v1` | `openai` | `claude` | yes | 80 | The patch applies cleanly, touches only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md, and leaves UBU-Q0128 Open with Resolved by: None and all metadata intact. It fills a real gap: UBU-D0309 through D0311 require a fresh unambiguous playback target but do not say how one is chosen or how 'pause that', 'go back', 'save this', 'what did I miss' and 'remind me about this' route. The answer is conservative and consistent with prior direction: explicit targets must resolve uniquely, implicit targets need a session selection plus fresh evidence and no competing audio, foreground/recency/model confidence cannot pick a target, referents clear on interruption or mode/endpoint/Device change, dispatch binds integration, target, control revision and endpoint and cannot fall back to a global active-player command, and every unsupported vague command defers or becomes unresolved evidence rather than an inferred effect. Local pause/stop/silence stopping UbU output immediately without inferring a second app effect is a sound safety property. The decision record, DESIGN text and OPEN_QUESTIONS current-direction and resolution updates agree with each other and correctly scope the contribution to subquestions 9, 14 and 15. Deductions are for unverifiable cross-references to v1 vocabulary, the density of the DESIGN insertion, and its placement ahead of the schema it depends on. No blocking fix is required; a reviewer should confirm `identify_playback_target` and `media_transport/pause` exist in the v1 closed set, and may prefer moving the section after the UBU-D0311 paragraph. | None | References `identify_playback_target` as the sole clarification kind and `media_transport/pause` as an action shape; neither is quoted in the prompt, so whether they exist in the closed `AudioInterpretationV1` vocabulary from UBU-D0310 is unverified. If `identify_playback_target` is not already in v1, the text contradicts its own claim that it does not expand `AudioInterpretationV1`., The DESIGN.md §21.1.2 insertion is dense (several long paragraphs plus a table and a fixture list), which weakens auditability of a decision that touches dispatch safety., The new section is placed between the UBU-D0309 controller-boundary paragraph and the UBU-D0310 schema paragraph, so it uses aliases and action names before the document defines them., UBU-D0322 uniqueness and the DECISIONS.md anchor are asserted by the author's validation notes only; mechanical validation confirms the patch applies but not ID uniqueness., Acceptance fixtures are stated as requirements rather than results, which is honest, but there is no pointer to where those fixtures would live or who gates adapter enablement. |

## Selected Summary

Define deterministic playback-target selection and vague-command routing within the existing restricted-attention audio envelope.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Acceptance fixtures are stated as requirements rather than results, which is honest, but there is no pointer to where those fixtures would live or who gates adapter enablement.
- Acceptance fixtures remain unexecuted, so this establishes a design contract rather than validated runtime behavior.
- Correct dispatch depends on adapters enforcing target and control-revision bindings; adapter support is not demonstrated.
- References `identify_playback_target` as the sole clarification kind and `media_transport/pause` as an action shape; neither is quoted in the prompt, so whether they exist in the closed `AudioInterpretationV1` vocabulary from UBU-D0310 is unverified. If `identify_playback_target` is not already in v1, the text contradicts its own claim that it does not expand `AudioInterpretationV1`.
- The DESIGN.md §21.1.2 insertion is dense (several long paragraphs plus a table and a fixture list), which weakens auditability of a decision that touches dispatch safety.
- The new section is placed between the UBU-D0309 controller-boundary paragraph and the UBU-D0310 schema paragraph, so it uses aliases and action names before the document defines them.
- UBU-D0322 uniqueness and the DECISIONS.md anchor are asserted by the author's validation notes only; mechanical validation confirms the patch applies but not ID uniqueness.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T122730Z-UBU-Q0128/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T122730Z-UBU-Q0128/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T122730Z-UBU-Q0128"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
