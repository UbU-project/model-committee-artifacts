# Model-Committee Review

Run: `20260917T032109Z-UBU-Q0130`  
Question: `UBU-Q0130`  
Base commit: `52539e0d38ef969bc76b00c2b7f11f4cb4c95e32`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0130-gpt5-codex-d0258`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0130-gpt5-codex-d0258` |
| `work-score` | `claude` | `sonnet` | `ubu-q0130-gpt5-codex-d0258` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0130-gpt5-codex-d0258` |
| `work-score` | `claude` | `sonnet` | `ubu-q0130-gpt5-codex-d0258` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0130-gpt5-codex-d0258` | `openai` | `codex` | yes | 88 | The proposal directly answers all six subquestions: it fixes the required envelope fields, represents observed_versions as per-object version references derived from existing integer counters including absence preconditions, defines an admission-owned causality/idempotency issuer seam, distinguishes created/effective/recorded time for delayed advisory admission, specifies deterministic replay behavior, and excludes derived artifacts unless admitted as canonical state. The patch applies, is limited to allowed documentation files, preserves the open-question metadata structure, and adds a focused decision record. The main residual concern is wording that may appear to assume a Phase 2 hybrid causality stack despite the selected question's warning not to settle UBU-Q0140 here. | None | The text references a Phase 2 hybrid-logical-clock/hybrid stack implementation, which could be read as leaning into the UBU-Q0140 ordering decision rather than leaving it fully deferred, although the main seam still keeps that choice out of Phase 1b call sites., The added observed_policy_versions requirement is plausible from the surrounding contract but slightly expands the field set beyond the question's enumerated mandatory fields. |
| `ubu-q0130-gpt5-codex-d0258` | `openai` | `claude` | yes | 89 | The proposal directly and completely answers all six subquestions: it freezes the exact mandated field set (idempotency_key, observed_versions, origin_device_id, actor_identity_id, authority_source, created/effective/recorded timestamps, plus observed_policy_versions when policy is relied on), represents observed_versions as a map of object-id to version-ref derived from the existing integer counter (v<N>, with explicit absence preconditions for creates), defines an admission-owned causality/idempotency issuer seam that explicitly excludes naming HLC/Lamport/vector clocks/logs/bundles at call sites, gives a concrete bitemporal example distinguishing effective_time and recorded_time for an overnight batch admitted the next morning, defines a deterministic duplicate rule keyed by (origin_device_id, idempotency_key) with same-payload replay vs. different-payload conflict, and correctly classifies derived artifacts as derived_state excluded from the envelope unless separately admitted as canonical. The patch is minimal, touching only documentation files consistent with a Data-model decision question, passes mechanical validation, and preserves the OPEN_QUESTIONS.md single-line metadata schema while adding a properly formatted DECISIONS.md entry (UBU-D0258) and DESIGN.md section. No decomposition into multiple questions/decisions was needed and none was introduced unnecessarily. | None | References UBU-D0247 ('hybrid stack') as an existing accepted decision that Phase 2 will implement the same seam with; this proposal's correctness partly depends on that decision already existing and matching the described HLC/causal-parent shape, which cannot be verified from this prompt alone., Sets Answerability score from 90 to 100 and Status to Solved despite the ticket still formally depending on UBU-Q0140 and UBU-Q0131 (both still open per the header); the proposal argues the causality mechanism itself is correctly deferred to UBU-Q0140, but marking the ticket fully 'Solved' rather than 'Solved pending dependency confirmation' is a modest overreach., The idempotency_key_conflict handling is asserted but no reference is made to how or where such conflicts are surfaced/logged, leaving an implicit gap for implementers. |

## Selected Summary

Resolve UBU-Q0130 by adding UBU-D0258, documenting the Phase 1b sync-ready canonical mutation envelope, and updating the selected open question to solved.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DESIGN.md`
- `DECISIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- References UBU-D0247 ('hybrid stack') as an existing accepted decision that Phase 2 will implement the same seam with; this proposal's correctness partly depends on that decision already existing and matching the described HLC/causal-parent shape, which cannot be verified from this prompt alone.
- Sets Answerability score from 90 to 100 and Status to Solved despite the ticket still formally depending on UBU-Q0140 and UBU-Q0131 (both still open per the header); the proposal argues the causality mechanism itself is correctly deferred to UBU-Q0140, but marking the ticket fully 'Solved' rather than 'Solved pending dependency confirmation' is a modest overreach.
- The added observed_policy_versions requirement is plausible from the surrounding contract but slightly expands the field set beyond the question's enumerated mandatory fields.
- The idempotency_key_conflict handling is asserted but no reference is made to how or where such conflicts are surfaced/logged, leaving an implicit gap for implementers.
- The text references a Phase 2 hybrid-logical-clock/hybrid stack implementation, which could be read as leaning into the UBU-Q0140 ordering decision rather than leaving it fully deferred, although the main seam still keeps that choice out of Phase 1b call sites.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260917T032109Z-UBU-Q0130/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260917T032109Z-UBU-Q0130/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260917T032109Z-UBU-Q0130"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
