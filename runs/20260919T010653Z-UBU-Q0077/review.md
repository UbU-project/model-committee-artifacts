# Model-Committee Review

Run: `20260919T010653Z-UBU-Q0077`  
Question: `UBU-Q0077`  
Base commit: `ef2593f0f9ee9be4ba16b661e144b81d936d2a43`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0077-openai-gpt-5-codex-20260919`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 84.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0077-openai-gpt-5-codex-20260919` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0077-openai-gpt-5-codex-20260919` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0077-openai-gpt-5-codex-20260919` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0077-openai-gpt-5-codex-20260919` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0077-openai-gpt-5-codex-20260919` | `openai` | `codex` | yes | 88 | The patch applies, touches only allowed documentation files, resolves the selected question with an auditable decision, and directly answers the representation, required Phase 1b candidate types, elapsed-time semantics, provenance/confidence, and sensitive routing questions. It preserves the open-question schema and correctly keeps realtime outputs outside admitted state. The main weakness is mild overbreadth: it includes future candidate classes and sensitive media capture policy details beyond the immediate Phase 1b minimum, but it gates them conservatively and does not appear to introduce contradictory implementation scope. | None | The schema is somewhat broad for the Phase 1b minimum, especially by naming later realtime candidate types and sensitive audio/video/screen/GPS routing rules in the accepted decision., Some fields such as routing mode, admission hint, revocation links, and prompt/template version are useful but may be more than strictly minimum unless aligned elsewhere in the design corpus., It references candidate lifecycle audit behavior that is decomposed to UBU-Q0138, so implementers must avoid treating this decision as fully specifying durable rejection or lifecycle semantics. |
| `UBU-Q0077-openai-gpt-5-codex-20260919` | `openai` | `claude` | yes | 80 | The patch applies cleanly, touches only allowlisted files, and answers all five subquestions. Its session model is a hybrid that is not a Task, Log or sensor stream. Model-noticed time is treated as evidence. Candidates stay outside admitted state. Local-only and cloud-optional routing is stated conservatively. The Phase 1b candidate types are named correctly. Its main weaknesses are scope creep beyond the Phase 1b re-scope and a missing handoff to Q0138. | Split the schema into a Phase 1b core (candidate-update record, five types, provenance and confidence) and a deferred section for realtime audio/video and AssociationAttestation candidates., Explicitly cite UBU-Q0138 as owning candidate lifecycle and durable rejection, and remove or narrow the lifecycle fields., Confirm that the D0272 ID and the §1.5 numbering are free of collisions. | The Phase 1b re-scope says the candidate-update schema is required now and independent of any realtime audio or video backend. The proposal still defines the full realtime session schema, including audio, video, screen and keystroke routing rules. That goes beyond the Phase 1b minimum., It lists the eight non-Phase-1b candidate types, including association_attestation_candidate, as 'allowed'. The re-scope calls that type a non-Phase-1b concern, so this could read as premature admission., The session and candidate records list many required fields, which may over-constrain Phase 1b before implementation experience., It does not reference UBU-Q0138 for candidate lifecycle and durable rejection. It instead mentions rejection, revocation and supersession links and says lifecycle rules apply, which risks overlapping with or pre-empting Q0138., The decision is marked Solved and Accepted although the question needs human approval. requires_human_review is true, which mitigates this., DESIGN.md §1.5 numbering and placement may collide with existing sections. I could not verify this. The patch does apply mechanically., The new decision ID UBU-D0272 was not checked for collision with existing IDs. |

## Selected Summary

Resolve UBU-Q0077 by adding a minimal realtime session and candidate-update schema, then marking realtime outputs as reviewable candidates rather than admitted state.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DESIGN.md`
- `DECISIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- DESIGN.md §1.5 numbering and placement may collide with existing sections. I could not verify this. The patch does apply mechanically.
- It does not reference UBU-Q0138 for candidate lifecycle and durable rejection. It instead mentions rejection, revocation and supersession links and says lifecycle rules apply, which risks overlapping with or pre-empting Q0138.
- It lists the eight non-Phase-1b candidate types, including association_attestation_candidate, as 'allowed'. The re-scope calls that type a non-Phase-1b concern, so this could read as premature admission.
- It references candidate lifecycle audit behavior that is decomposed to UBU-Q0138, so implementers must avoid treating this decision as fully specifying durable rejection or lifecycle semantics.
- Some fields such as routing mode, admission hint, revocation links, and prompt/template version are useful but may be more than strictly minimum unless aligned elsewhere in the design corpus.
- The Phase 1b re-scope says the candidate-update schema is required now and independent of any realtime audio or video backend. The proposal still defines the full realtime session schema, including audio, video, screen and keystroke routing rules. That goes beyond the Phase 1b minimum.
- The decision is marked Solved and Accepted although the question needs human approval. requires_human_review is true, which mitigates this.
- The new decision ID UBU-D0272 was not checked for collision with existing IDs.
- The schema is somewhat broad for the Phase 1b minimum, especially by naming later realtime candidate types and sensitive audio/video/screen/GPS routing rules in the accepted decision.
- The session and candidate records list many required fields, which may over-constrain Phase 1b before implementation experience.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260919T010653Z-UBU-Q0077/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260919T010653Z-UBU-Q0077/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260919T010653Z-UBU-Q0077"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
