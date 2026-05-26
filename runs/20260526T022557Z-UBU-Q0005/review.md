# Model-Committee Review

Run: `20260526T022557Z-UBU-Q0005`  
Question: `UBU-Q0005`  
Base commit: `ac1490c309a0ecbb11a7e04d4bf96bd20742bd18`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0005-github-event-triage-gpt5-20260525`

## Disagreement Flags

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
| `work-score` | `claude` | `sonnet` | `ubu-q0005-github-event-triage-gpt5-20260525` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `ubu-q0005-github-event-triage-gpt5-20260525` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0005-github-event-triage-gpt5-20260525` | `codex` | `claude` | yes | 88 | The patch applies cleanly per mechanical validation. It addresses all nine sub-questions of UBU-Q0005 directly and systematically: four log-only cases are enumerated, the default External Event rule is stated clearly, per-event-class triage rules cover all eleven candidate event classes, Objective and Task creation thresholds are defined, recalculation reuses the existing `github_update` trigger kind, worker assignment is correctly kept Task-driven rather than event-driven, projection updates are kept projection-driven, and duplicate detection uses a two-layer scheme with explicit fallback key composition. The OPEN_QUESTIONS.md status line, resolution field, and body are updated in the correct locations without disturbing surrounding content. DECISIONS.md adds UBU-D0163 with full consequences, and DESIGN.md gains §26.5 as a coherent subsection following existing §26.x structure. Scope is appropriately bounded: UBU-Q0006 (noise control), UBU-Q0004 (pipeline state), and UBU-Q0010 (token custody) remain open. The main weaknesses are the qualifier-heavy per-event rules that could produce implementation ambiguity and the unmitigated event-volume risk deferred to Q0006, neither of which is a blocking defect for Phase 1 resolution. | None | The blanket rule that every unique in-scope GitHub observation creates an External Event could produce high event volume in busy repositories; the deferral to UBU-Q0006 for noise-control means this risk is acknowledged but not yet mitigated., Qualifier language ('usually', 'when possible', 'only when') in per-event-class defaults is intentionally flexible but may require repeated interpretation during implementation, potentially leading to inconsistent behavior across importers., The composite fallback deduplication key (repository, class, action, object type/ID, actor, timestamp, version, UbU target) is complex; hash-collision or clock-skew edge cases are not addressed., Objective creation criteria ('durable desired state') are left somewhat open to interpreter judgment, which could cause drift between implementations. |

## Selected Summary

Resolve GitHub event triage rules for Phase 1 by defining log-only cases, External Event defaults, Objective and Task creation rules, recalculation behavior, worker-assignment boundaries, projection update triggers, duplicate detection, and reconciliation reconstruction.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Objective creation criteria ('durable desired state') are left somewhat open to interpreter judgment, which could cause drift between implementations.
- Qualifier language ('usually', 'when possible', 'only when') in per-event-class defaults is intentionally flexible but may require repeated interpretation during implementation, potentially leading to inconsistent behavior across importers.
- The blanket rule that every unique in-scope GitHub observation creates an External Event could produce high event volume in busy repositories; the deferral to UBU-Q0006 for noise-control means this risk is acknowledged but not yet mitigated.
- The composite fallback deduplication key (repository, class, action, object type/ID, actor, timestamp, version, UbU target) is complex; hash-collision or clock-skew edge cases are not addressed.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T022557Z-UBU-Q0005/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T022557Z-UBU-Q0005/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T022557Z-UBU-Q0005"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
