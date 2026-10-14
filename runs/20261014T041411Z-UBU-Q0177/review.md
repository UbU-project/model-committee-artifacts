# Model-Committee Review

Run: `20261014T041411Z-UBU-Q0177`  
Question: `UBU-Q0177`  
Base commit: `5c241d7b051f65cb1afcee92038d2dc80b788cc9`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0177-openai-runtime-isolation-20261014`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 85.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0177-openai-runtime-isolation-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0177-openai-runtime-isolation-20261014` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0177-openai-runtime-isolation-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0177-openai-runtime-isolation-20261014` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0177-openai-runtime-isolation-20261014` | `openai` | `codex` | yes | 91 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch adds concrete, auditable isolation fixtures, authorized controls and measurements with protections enabled. It connects directly to secure hardware qualification while retaining existing authority boundaries, human approval and Open status. Its limitation is evidentiary: it strengthens the protocol without supplying the measured hardware answer. | None | No real-device measurements establish a sufficient hardware profile; the proposal advances qualification requirements only., Isolation evidence remains bounded by fixture coverage and runtime observability, which must be demonstrated during qualification. |
| `UBU-Q0177-openai-runtime-isolation-20261014` | `openai` | `claude` | yes | 80 | Patch passed mechanical validation and touches only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md. UBU-Q0177 metadata line, Status: Open and Resolution semantics are preserved; the question is only annotated to reference UBU-D0336. The work is directly responsive to the 'securely' dimension of the question: cross-job runtime state (KV caches, decoded media, handles, scratch) is a plausible leak path in a warm local multimodal runtime, and the proposal makes it an explicit, auditable gate inside the existing UBU-D0331 protocol, including a positive control so detector failure is not mistaken for isolation. Requiring cold/warm and sustained measurements to be re-run with protections enabled prevents benchmarking an unsafe configuration, which is the most concrete improvement to the eventual hardware profile. The proposal correctly declines to select hardware or close the question. Points withheld for the unverified §21.1.5 anchor, the added protocol weight without any new evidence, and the density of the inserted text. | None | New DESIGN.md text cites §21.1.5 (artifact lifecycle / broker checks); the cross-reference is asserted by the author and not independently verified here. If that anchor does not exist at the base commit the decision text dangles., The proposal extends the qualification protocol rather than supplying measurements. Each added mandatory gate raises the cost of actually closing UBU-Q0177, which already cannot close without real-device evidence and operator approval., UBU-D0336 is self-labelled Accepted while the question is Human-approval-required; the proposal correctly sets requires_human_review, but the decision number and Accepted status may collide with concurrently authored decisions and must be rechecked at merge time., The DESIGN.md addition is three dense paragraphs; the volume of fixture detail (job orders, revocation, crash/restart, resource pressure, concurrency, positive control) is reasonable for a security gate but increases the risk that later editors cannot tell which items are mandatory versus conditional. |

## Selected Summary

Add cross-job inference-runtime isolation fixtures and measure their costs when qualifying secure local multimodal hardware.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Isolation evidence remains bounded by fixture coverage and runtime observability, which must be demonstrated during qualification.
- New DESIGN.md text cites §21.1.5 (artifact lifecycle / broker checks); the cross-reference is asserted by the author and not independently verified here. If that anchor does not exist at the base commit the decision text dangles.
- No real-device measurements establish a sufficient hardware profile; the proposal advances qualification requirements only.
- The DESIGN.md addition is three dense paragraphs; the volume of fixture detail (job orders, revocation, crash/restart, resource pressure, concurrency, positive control) is reasonable for a security gate but increases the risk that later editors cannot tell which items are mandatory versus conditional.
- The proposal extends the qualification protocol rather than supplying measurements. Each added mandatory gate raises the cost of actually closing UBU-Q0177, which already cannot close without real-device evidence and operator approval.
- UBU-D0336 is self-labelled Accepted while the question is Human-approval-required; the proposal correctly sets requires_human_review, but the decision number and Accepted status may collide with concurrently authored decisions and must be rechecked at merge time.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T041411Z-UBU-Q0177/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T041411Z-UBU-Q0177/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T041411Z-UBU-Q0177"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
