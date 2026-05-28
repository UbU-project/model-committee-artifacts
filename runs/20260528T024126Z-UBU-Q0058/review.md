# Model-Committee Review

Run: `20260528T024126Z-UBU-Q0058`  
Question: `UBU-Q0058`  
Base commit: `0b7464f224d020ee3721b543fdc87f548e08a97c`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0058-adaptive-granularity-001`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0058-adaptive-granularity-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0058-adaptive-granularity-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0058-adaptive-granularity-001` | `codex` | `claude` | yes | 88 | Patch applies cleanly and is mechanically valid. It directly answers all six subquestions from UBU-Q0058: presets vs. constants (subq 1), required explanation content (subq 2), full trigger enumeration (subq 3), precompute bounds and scope (subq 4), cache expiry conditions (subq 5), and the minimum mobile-only guarantee (subq 6). The DECISIONS.md entry UBU-D0203 is well-structured with clear consequences. The DESIGN.md replacement is substantive and replaces vague provisional text with concrete, auditable policy. The OPEN_QUESTIONS.md tombstone correctly marks the question solved and references the decision record. No new schema fields, trigger kinds, or out-of-scope Phase 1 features are introduced. Minor deductions for the tombstone formatting, the indistinguishable offline_steward vs. mobile_low_power profiles, and the absence of expiry-trigger priority ordering. | None | The tombstone in OPEN_QUESTIONS.md drops the heading separator blank line before the status line, which may cause minor markdown rendering inconsistency relative to other tombstoned questions in the file., The offline_steward and mobile_low_power profiles share identical numeric values (900s delta, 1800s horizon, 0.95 coverage) with no distinguishing runtime constraint — a future reader may not know when to prefer one over the other., The cache-expiry rules list many independent triggers but provide no priority ordering or conflict-resolution rule when multiple triggers fire simultaneously, leaving ambiguity for implementors. |

## Selected Summary

Resolve UBU-Q0058 by replacing provisional Compact Calendar granularity text with explicit execution-profile presets, offline precompute behavior, cache expiration rules, and the minimum mobile-only guarantee.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- The cache-expiry rules list many independent triggers but provide no priority ordering or conflict-resolution rule when multiple triggers fire simultaneously, leaving ambiguity for implementors.
- The offline_steward and mobile_low_power profiles share identical numeric values (900s delta, 1800s horizon, 0.95 coverage) with no distinguishing runtime constraint — a future reader may not know when to prefer one over the other.
- The tombstone in OPEN_QUESTIONS.md drops the heading separator blank line before the status line, which may cause minor markdown rendering inconsistency relative to other tombstoned questions in the file.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T024126Z-UBU-Q0058/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T024126Z-UBU-Q0058/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T024126Z-UBU-Q0058"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
