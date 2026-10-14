# Model-Committee Review

Run: `20261014T104703Z-UBU-Q0177`  
Question: `UBU-Q0177`  
Base commit: `6aa6f349da372a12a3e8e514dd0468d4547d6f3d`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0177-openai-20261014-complete-local-route`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 84.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0177-openai-20261014-complete-local-route` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0177-openai-20261014-complete-local-route` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0177-openai-20261014-complete-local-route` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0177-openai-20261014-complete-local-route` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0177-openai-20261014-complete-local-route` | `openai` | `codex` | yes | 94 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch directly strengthens secure hardware qualification by covering delegated processing, connected-mode behavior, retries, enforcement and complete-pipeline costs. Changes remain confined to the qualification protocol and its decision/question records. It preserves human approval, mandatory-slice coverage and the question's Open status without asserting unsupported results. No decomposition or blocking defect is evident. | None | Opaque delegated OS services may complicate implementation pinning and observation, requiring instrumentation or exclusion of affected modes., Actual hardware qualification remains outstanding; this protocol extension supplies no measured minimum or supported configuration. |
| `ubu-q0177-openai-20261014-complete-local-route` | `openai` | `claude` | yes | 74 | Patch applies and passes allowlist and question-schema validation. Changes are confined to DECISIONS.md (new UBU-D0368), a DESIGN.md §1 insertion placed sensibly between the D0362 and D0332 paragraphs, and UBU-Q0177's Current direction and Resolution sections with the metadata line and Status: Open untouched. The content is directly responsive to the question's 'secure local' and 'endpoint security and local storage' direction: it closes a genuine gap where OS-delegated speech, vision or speech-output services could route source content off-device even when the main inference process is egress-denied, and it correctly declines to claim any supported hardware or passing route. The decision honestly keeps the question Open and flags human review. Deductions for adding another gate layer without any progress toward real-device evidence, for the unaddressed consequence on platforms where delegated-service traffic is unobservable, and for density and partial redundancy with existing gates. | None | Protocol accretion: this is the fourth narrowing decision on UBU-Q0177 (after D0331, D0336, D0362) and still supplies no measured evidence, hardware, or evidence-collection owner; the question moves no closer to closure., The rule that an unobserved required path withholds the affected mode may be unsatisfiable on locked-down mobile platforms where OS-delegated speech/vision service traffic cannot be inspected, which could silently exclude primary mobile interaction; the text does not say whether that outcome is intended., Partial overlap with D0331 endpoint/storage gates and D0362 qualified-endpoint/worker-route requirements; the new DESIGN.md paragraphs restate rather than cross-reference some of those gates., The DESIGN.md addition is three dense paragraphs of enforcement obligations (connected/blocked/backend-loss/reconnection conditions, denied-egress probe, worker-transfer control, retention costs, change rechecks); auditability of the support matrix grows harder with each layer., References to 'frozen useful-outcome fixtures' and 'the existing manual route' assume those artifacts are already defined in DESIGN.md; could not verify against the UbU repository from this environment, only via the mechanical validation result. |

## Selected Summary

Require complete-pipeline locality evidence, including delegated OS services and reconnection behavior, before approving multimodal hardware profiles.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Actual hardware qualification remains outstanding; this protocol extension supplies no measured minimum or supported configuration.
- Opaque delegated OS services may complicate implementation pinning and observation, requiring instrumentation or exclusion of affected modes.
- Partial overlap with D0331 endpoint/storage gates and D0362 qualified-endpoint/worker-route requirements; the new DESIGN.md paragraphs restate rather than cross-reference some of those gates.
- Protocol accretion: this is the fourth narrowing decision on UBU-Q0177 (after D0331, D0336, D0362) and still supplies no measured evidence, hardware, or evidence-collection owner; the question moves no closer to closure.
- References to 'frozen useful-outcome fixtures' and 'the existing manual route' assume those artifacts are already defined in DESIGN.md; could not verify against the UbU repository from this environment, only via the mechanical validation result.
- The DESIGN.md addition is three dense paragraphs of enforcement obligations (connected/blocked/backend-loss/reconnection conditions, denied-egress probe, worker-transfer control, retention costs, change rechecks); auditability of the support matrix grows harder with each layer.
- The rule that an unobserved required path withholds the affected mode may be unsatisfiable on locked-down mobile platforms where OS-delegated speech/vision service traffic cannot be inspected, which could silently exclude primary mobile interaction; the text does not say whether that outcome is intended.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T104703Z-UBU-Q0177/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T104703Z-UBU-Q0177/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T104703Z-UBU-Q0177"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
