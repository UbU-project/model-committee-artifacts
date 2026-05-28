# Model-Committee Review

Run: `20260528T005908Z-UBU-Q0107`  
Question: `UBU-Q0107`  
Base commit: `544297d16f71875d66b225238fe10f29c31a7b8b`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0107-counterfactual-logging-001`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 85.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0107-counterfactual-logging-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0107-counterfactual-logging-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0107-counterfactual-logging-001` | `codex` | `claude` | yes | 85 | The proposal cleanly resolves all five UBU-Q0107 subquestions in the correct files. Decision kinds are enumerated, the reason_capture struct elegantly avoids making explanation mandatory while preserving evidence, append-only correction semantics are explicitly stated, and preference inference constraints are included. DECISIONS.md entry UBU-D0198 mirrors the DESIGN.md content appropriately. The tombstone in OPEN_QUESTIONS.md follows the established pattern (e.g., §UBU-Q0106) and preserves the heading. Two fields — decision_surface and preference_inference_allowed — are listed without normative definition, which leaves ambiguity for implementers. The omission of the §17.1 cross-reference is a minor coherence gap. Neither issue is blocking, but both should be addressed before implementation. | Define decision_surface with an enumerated set of values or a normative description, Add at least one sentence specifying when preference_inference_allowed must be set to false | decision_surface field is listed in the minimum payload but never defined — implementers have no guidance on expected values or enumeration, preference_inference_allowed is listed as a field but no rules are given for when it should be false, leaving the gate condition underspecified, presented_at duplicates or conflicts with the existing Log entry timestamp; the relationship is not clarified, The tombstone removes all five subquestions from OPEN_QUESTIONS.md, which loses reviewable traceability of what was actually asked; future readers must cross-reference DECISIONS.md to reconstruct the question scope, No cross-reference added from DESIGN.md §17.0 to §17.1 to show how decision_recorded slots into the shared MVP event envelope already specified there |

## Selected Summary

Resolve UBU-Q0107 by specifying MVP counterfactual logging as structured decision_recorded payloads with required decision kinds, optional reason capture, system-state refs, append-only correction semantics, and preference-inference limits.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- No cross-reference added from DESIGN.md §17.0 to §17.1 to show how decision_recorded slots into the shared MVP event envelope already specified there
- The tombstone removes all five subquestions from OPEN_QUESTIONS.md, which loses reviewable traceability of what was actually asked; future readers must cross-reference DECISIONS.md to reconstruct the question scope
- decision_surface field is listed in the minimum payload but never defined — implementers have no guidance on expected values or enumeration
- preference_inference_allowed is listed as a field but no rules are given for when it should be false, leaving the gate condition underspecified
- presented_at duplicates or conflicts with the existing Log entry timestamp; the relationship is not clarified

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T005908Z-UBU-Q0107/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T005908Z-UBU-Q0107/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T005908Z-UBU-Q0107"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
