# Model-Committee Review

Run: `20261014T044120Z-UBU-Q0183`  
Question: `UBU-Q0183`  
Base commit: `f4f9136498666249e8f78c7c1ffb410fce4eb69b`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-6-UBU-Q0183-D0339`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-gpt-6-UBU-Q0183-D0339` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-gpt-6-UBU-Q0183-D0339` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-gpt-6-UBU-Q0183-D0339` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-gpt-6-UBU-Q0183-D0339` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-6-UBU-Q0183-D0339` | `openai` | `codex` | yes | 95 | Mechanical validation passes and changes stay within allowed files. The proposal explicitly orders mandatory desktop, phone and genuine streaming slices, preserves consent and retention gates, and coordinates release with Phase 2 and the deferred switch. It correctly uses Status: Solved while preserving scoring metadata. Mobile stewardship, switch criteria and unresolved measured qualification remain intact; no blocking fixes were found. | None | Mandatory phone capture and desktop streaming make release dependent on outstanding host-mapping, hardware, latency, retention and cost qualification. |
| `openai-gpt-6-UBU-Q0183-D0339` | `openai` | `claude` | yes | 80 | Patch applies cleanly, touches only allowlisted files, and passes question-schema checks (Status: Solved, Resolved by: UBU-D0339, remaining metadata preserved). It answers exactly what UBU-Q0183 asks: an ordered set of mandatory mode/device slices (bounded desktop snapshot+PTT voice, bounded phone capture/review, consented finite desktop streaming), explicit evidence and retention gates before each enablement, coordination with Phase 2 and the deferred switch, and a clear list of what is excluded (phone streaming, always-on, Phase 3 gate). It honors the stated constraints: no Phase 3 release gate restored, always-on consent never inferred, mobile stewardship left to UBU-Q0160, switch criteria unchanged, no implementation or hardware-support claim. Cross-references in DESIGN.md §1 and §21.1.3 are updated coherently. Deductions are for unverifiable specific citations (IDs and the 60-second figure), the unreconciled change to the UBU-Q0128 phase statement, assumptions about Phase 1b closure, and the substantial length/duplication of gate text in §4.2. These are auditable fixes rather than structural flaws. | Verify every cited decision/question ID and the 60-second §12.2 raw-buffer figure exist at base commit f4f9136; remove or correct any that do not., Confirm UBU-Q0128's current Phase tag and either keep the original 're-tagged Phase 1b' wording or update UBU-Q0128 consistently (outside this proposal if the allowlist forbids it)., Confirm that Phase 1b closure and the deferred switch are already recorded under UBU-D0305/UBU-D0291; otherwise soften the closure/reopening language to match the recorded roadmap state., Operator must explicitly approve the mandatory phone slice (row 2) as a release prerequisite, since it binds MVP release to Phase 2 route qualification. | The DESIGN.md §4.2 text cites many specific prior artifacts (UBU-D0326, D0327–D0328, D0331, D0332, D0336, D0337, UBU-Q0178) and a concrete '60 seconds' raw-buffer ceiling attributed to §12.2. None of these can be verified from the diff context; a wrong ID or number would introduce a false cross-reference into canonical design text., The rewritten amendment paragraph changes the statement about UBU-Q0128 from 're-tagged Phase 1b' to 'Phase 1b-origin work now tracked in Phase 2' without touching UBU-Q0128's own metadata. If UBU-Q0128 is still tagged Phase 1b in OPEN_QUESTIONS.md, this creates an inconsistency the patch does not reconcile., The decision repeatedly assumes Phase 1b is closed ('reopening Phase 1b', 'reversing Phase 1b closure', 'the deferred switch'). If the base commit does not record Phase 1b closure or a deferred switch, the sequencing language asserts roadmap state that was not decided here., Mandating a qualified phone capture/review profile (row 2) before MVP release is a material scope choice that depends on Phase 2 personal-worker/cross-Device route gates; it enlarges release risk relative to a desktop-only minimum and should be an explicit operator call., Mode/class identifiers (off/text_only, snapshot_capture, conversational, streamed_supervision, voice_session, discovery_mode, local_only_realtime) are used as if they are existing enumerations; if any is not defined in §21.1.x, the table introduces undefined terms., The added §4.2 block is long and dense; it duplicates gate language already present in §21.1.1–21.1.5 and §12.2 rather than cross-referencing, increasing drift risk between sections. |

## Selected Summary

Sequence bounded desktop capture, phone capture/review and explicitly consented desktop AV streaming before MVP release, with existing evidence gates and post-switch acceptance.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Mandating a qualified phone capture/review profile (row 2) before MVP release is a material scope choice that depends on Phase 2 personal-worker/cross-Device route gates; it enlarges release risk relative to a desktop-only minimum and should be an explicit operator call.
- Mandatory phone capture and desktop streaming make release dependent on outstanding host-mapping, hardware, latency, retention and cost qualification.
- Mode/class identifiers (off/text_only, snapshot_capture, conversational, streamed_supervision, voice_session, discovery_mode, local_only_realtime) are used as if they are existing enumerations; if any is not defined in §21.1.x, the table introduces undefined terms.
- The DESIGN.md §4.2 text cites many specific prior artifacts (UBU-D0326, D0327–D0328, D0331, D0332, D0336, D0337, UBU-Q0178) and a concrete '60 seconds' raw-buffer ceiling attributed to §12.2. None of these can be verified from the diff context; a wrong ID or number would introduce a false cross-reference into canonical design text.
- The added §4.2 block is long and dense; it duplicates gate language already present in §21.1.1–21.1.5 and §12.2 rather than cross-referencing, increasing drift risk between sections.
- The decision repeatedly assumes Phase 1b is closed ('reopening Phase 1b', 'reversing Phase 1b closure', 'the deferred switch'). If the base commit does not record Phase 1b closure or a deferred switch, the sequencing language asserts roadmap state that was not decided here.
- The rewritten amendment paragraph changes the statement about UBU-Q0128 from 're-tagged Phase 1b' to 'Phase 1b-origin work now tracked in Phase 2' without touching UBU-Q0128's own metadata. If UBU-Q0128 is still tagged Phase 1b in OPEN_QUESTIONS.md, this creates an inconsistency the patch does not reconcile.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T044120Z-UBU-Q0183/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T044120Z-UBU-Q0183/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T044120Z-UBU-Q0183"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
