# Model-Committee Review

Run: `20260916T183343Z-UBU-Q0148`  
Question: `UBU-Q0148`  
Base commit: `bc52aaa5d6a3bc77abb095616329ebb349c3acad`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0148-gpt5-codex-20260916`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0148-gpt5-codex-20260916` |
| `work-score` | `claude` | `sonnet` | `ubu-q0148-gpt5-codex-20260916` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0148-gpt5-codex-20260916` |
| `work-score` | `claude` | `sonnet` | `ubu-q0148-gpt5-codex-20260916` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0148-gpt5-codex-20260916` | `openai` | `codex` | yes | 92 | The proposal directly resolves UBU-Q0148 with a clear data-model distinction between physical machines, registered UbU Devices/execution enclaves, installs, sessions, and worker processes. It applies cleanly, modifies only the expected design, decisions, and question-tracking files, preserves the OPEN_QUESTIONS metadata shape, and adds a concise accepted decision linked back to the design. The scope is appropriately narrow and the answer is auditable. Minor ambiguity remains around listing browser sessions and worker runtimes as examples, but the surrounding constraints substantially mitigate that risk. | None | The DESIGN examples add browser profile/session and worker runtime as possible Device examples, which could be read too broadly despite later qualifying text that projection-only sessions and child workers are not Devices., The decision defines operational criteria but leaves some future registry details, such as exact isolation thresholds and Identity authorization mechanics, to later implementation. |
| `ubu-q0148-gpt5-codex-20260916` | `openai` | `claude` | yes | 88 | Patch applies cleanly and touches only DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md, consistent with the mechanical validation. It directly answers the question by defining Device as a registered execution enclave distinct from physical machine/install/browser session/worker, and specifies concrete conditions under which one machine can host multiple Devices, which is exactly what the question asks. It preserves the OPEN_QUESTIONS.md single-line metadata format, correctly flips Status to Solved and sets Resolved by, and appends a properly formatted new decision entry to DECISIONS.md consistent with existing numbering. Scope is proportionate to the question (a data-model clarification), not overreaching into implementation. Minor risk is that it doesn't explicitly cross-reference or update DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4/§5, the cited defining context, though this may be acceptable since DESIGN.md §23.1 is the canonical location. | None | Introduces a new normative decision (UBU-D0251) that downstream questions (e.g. UBU-Q0131, Phase 2 Device registry) will need to be checked against for consistency, The 'app installs/browser sessions are projection surfaces unless independently registered' distinction is a substantive policy call that may need cross-checking against DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4/§5, which is the defining context but not shown as modified |

## Selected Summary

Resolve UBU-Q0148 by defining UbU Device identity as a registered execution enclave, not a physical machine, install, browser session, or process. Clarify when one physical machine may host multiple Devices and when app/browser/worker contexts remain projection or child execution contexts instead.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Introduces a new normative decision (UBU-D0251) that downstream questions (e.g. UBU-Q0131, Phase 2 Device registry) will need to be checked against for consistency
- The 'app installs/browser sessions are projection surfaces unless independently registered' distinction is a substantive policy call that may need cross-checking against DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4/§5, which is the defining context but not shown as modified
- The DESIGN examples add browser profile/session and worker runtime as possible Device examples, which could be read too broadly despite later qualifying text that projection-only sessions and child workers are not Devices.
- The decision defines operational criteria but leaves some future registry details, such as exact isolation thresholds and Identity authorization mechanics, to later implementation.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260916T183343Z-UBU-Q0148/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260916T183343Z-UBU-Q0148/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260916T183343Z-UBU-Q0148"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
