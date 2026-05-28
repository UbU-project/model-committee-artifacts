# Model-Committee Review

Run: `20260528T022244Z-UBU-Q0057`  
Question: `UBU-Q0057`  
Base commit: `c73e4cfe3a1f0a29e90c41b45805c99a2e3146f0`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0057-evergreen-gap-fillers-v1`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0057-evergreen-gap-fillers-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0057-evergreen-gap-fillers-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0057-evergreen-gap-fillers-v1` | `codex` | `claude` | yes | 87 | Patch applies cleanly and is mechanically validated. The proposal correctly answers all six subquestions: gap-fillers are a Task property (gap_fill_policy) rather than a new subtype, required and optional fields are enumerated, ranking semantics are specified, autonomy protections are explicit, reactive-layer integration is described, and Phase 1 vs future scope is clearly delineated. DESIGN.md sections 9.3.1, 15.4, 16.4, and 16.6 are updated coherently. UBU-D0202 in DECISIONS.md is well-formed with consequences. The main weakness is the OPEN_QUESTIONS.md collapse: the structured header with all metadata fields is replaced by a one-line status, which loses the schema-required field inventory even though Resolved by and Last scored are present inline. This is a minor schema preservation issue. No unnecessary scope is introduced — no new WorkItem subtypes, no planning kernel contract changes, no Technique dependencies. The prose is precise and internally consistent. | OPEN_QUESTIONS.md should retain the structured question header block with Resolved by and resolution text rather than collapsing to a single status line — the schema requires the full header fields | OPEN_QUESTIONS.md entry is collapsed to two lines, losing the structured subquestion and field inventory that could be useful for future readers cross-referencing resolution context, gap_fill_policy is introduced as a named field bundle but no schema stub or JSON Schema fragment is provided, leaving implementers to infer field types and validation rules from prose alone, rank_hint as an optional field for local tie-breaking is underspecified — without a type or range it is ambiguous whether this is a float weight, an enum, or a user-settable priority level, cooldown_policy with explicit none_declared sentinel value is novel semantics not defined elsewhere in DESIGN.md; may conflict with existing precondition or recurrence patterns |

## Selected Summary

Resolve UBU-Q0057 by modeling evergreen gap-fillers as ordinary Dynamic Tasks with a Phase 1 gap_fill_policy, suggestion-only gap behavior, and reactive early-completion handling.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- OPEN_QUESTIONS.md entry is collapsed to two lines, losing the structured subquestion and field inventory that could be useful for future readers cross-referencing resolution context
- cooldown_policy with explicit none_declared sentinel value is novel semantics not defined elsewhere in DESIGN.md; may conflict with existing precondition or recurrence patterns
- gap_fill_policy is introduced as a named field bundle but no schema stub or JSON Schema fragment is provided, leaving implementers to infer field types and validation rules from prose alone
- rank_hint as an optional field for local tie-breaking is underspecified — without a type or range it is ambiguous whether this is a float weight, an enum, or a user-settable priority level

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T022244Z-UBU-Q0057/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T022244Z-UBU-Q0057/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T022244Z-UBU-Q0057"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
