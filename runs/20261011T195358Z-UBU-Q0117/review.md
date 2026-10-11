# Model-Committee Review

Run: `20261011T195358Z-UBU-Q0117`  
Question: `UBU-Q0117`  
Base commit: `f5ed2d2d0ca010e458a87dd2a395f05cc6278c50`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-6-UBU-Q0117-20261011`

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
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-gpt-6-UBU-Q0117-20261011` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-gpt-6-UBU-Q0117-20261011` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-gpt-6-UBU-Q0117-20261011` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-gpt-6-UBU-Q0117-20261011` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-6-UBU-Q0117-20261011` | `openai` | `codex` | yes | 96 | Provided mechanical validation passes, and the repository matches the stated base commit. The patch answers all five subquestions, preserves question metadata with Status: Solved, and changes only allowed files. It consistently uses existing moot reasons, pairwise Preferences, CPU planning boundaries, routine controls, and append-only admission rules. Review scheduling and explicit actions are auditable; the identified edge cases do not block acceptance. | None | Local-date review boundaries leave handling of ambiguous or nonexistent local midnight implicit., Promotion requires a comparable Task or Objective for an accepted pairwise Preference, which can delay activation of isolated ideas. |
| `openai-gpt-6-UBU-Q0117-20261011` | `openai` | `claude` | yes | 86 | The patch applies cleanly, touches only the three allowlisted files, and sets `Status: Solved` with `Resolved by: UBU-D0308` while preserving all other question metadata. It answers all five subquestions directly: (1) a single `deferred_intent` record with episode id, entry time, optional reason, optional tagged defer-until boundary, interval, last/next review, and an explicit user deferral action; (2) full exclusion before kernel dispatch including gap-fill and transitive dependents, i.e. the 'invisible' option, with no TaskSpec change; (3) new `task_deferred_intent_entered/updated/exited` events plus `decision_recorded` with `decision_kind: deferred_intent_review`; (4) a 30-day default surfaced on Log review or startup, postponed by defer_until, pulled earlier by material linked-Objective changes, with manual review always available; (5) promote, keep deferred, dismiss via `moot`, or Objective review. It follows the structure of the adjacent §9.6/UBU-D0307 waiting lifecycle, appends a single new decision in sequence, allocates no new questions, and updates the §17.8 Log-review bullet coherently. The current direction (Preference requirement enforced at review rather than creation) is honoured. Deductions are for unverifiable cross-references, the `cancelled`-to-`moot` reinterpretation, the promotion-stricter-than-creation asymmetry, and the density of the new section, which hurts auditability. | None | §9.7 cites many existing constructs that cannot be verified from the prompt alone (§9.5 moot reason codes `no_longer_relevant`, `invalidated_by_universe_change`, `user_declared_moot`; §7.4.1 routine occurrence controls; §17.0 decision payload; §17.3 correction links; `elapsed_time`/`log_review_due` paths; Setting vs Preference distinction). Any of these being absent or named differently would leave dangling references., The proposal maps the question's 'cancel permanently' option onto the existing terminal `moot` status and explicitly forbids a `cancelled` status, while the replaced DESIGN.md paragraph previously spoke of 'cancelled (which is terminal)'. If the Task model actually has a distinct cancelled status, this introduces a contradiction rather than a clarification., Promotion to active requires citing an enabled, user-accepted pairwise Preference. If ordinary Task admission does not require a Preference at creation, promoted Tasks are held to a stricter bar than newly created active Tasks, which is an asymmetry the text does not acknowledge., The §9.7 text is very dense (idempotency keys, sync coalescing, Compartment/Replica caveats, multi-clause `next_review_at` recomputation rule). It is hard to audit and some sentences are difficult to parse, raising the chance of later misreading or conflicting interpretation., Several implementation-flavoured details (exact idempotency key tuples, 2592000-second default, per-episode coalescing) are specified in a design document; they are consistent with the §9.6/UBU-D0307 pattern but lock in choices that may better belong to a later implementation decision. |

## Selected Summary

Complete the Phase 2 deferred-intent lifecycle, planner exclusion, review cadence, dismissal actions, and audit events; resolve UBU-Q0117 with UBU-D0308.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Local-date review boundaries leave handling of ambiguous or nonexistent local midnight implicit.
- Promotion requires a comparable Task or Objective for an accepted pairwise Preference, which can delay activation of isolated ideas.
- Promotion to active requires citing an enabled, user-accepted pairwise Preference. If ordinary Task admission does not require a Preference at creation, promoted Tasks are held to a stricter bar than newly created active Tasks, which is an asymmetry the text does not acknowledge.
- Several implementation-flavoured details (exact idempotency key tuples, 2592000-second default, per-episode coalescing) are specified in a design document; they are consistent with the §9.6/UBU-D0307 pattern but lock in choices that may better belong to a later implementation decision.
- The proposal maps the question's 'cancel permanently' option onto the existing terminal `moot` status and explicitly forbids a `cancelled` status, while the replaced DESIGN.md paragraph previously spoke of 'cancelled (which is terminal)'. If the Task model actually has a distinct cancelled status, this introduces a contradiction rather than a clarification.
- The §9.7 text is very dense (idempotency keys, sync coalescing, Compartment/Replica caveats, multi-clause `next_review_at` recomputation rule). It is hard to audit and some sentences are difficult to parse, raising the chance of later misreading or conflicting interpretation.
- §9.7 cites many existing constructs that cannot be verified from the prompt alone (§9.5 moot reason codes `no_longer_relevant`, `invalidated_by_universe_change`, `user_declared_moot`; §7.4.1 routine occurrence controls; §17.0 decision payload; §17.3 correction links; `elapsed_time`/`log_review_due` paths; Setting vs Preference distinction). Any of these being absent or named differently would leave dangling references.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261011T195358Z-UBU-Q0117/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261011T195358Z-UBU-Q0117/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261011T195358Z-UBU-Q0117"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
