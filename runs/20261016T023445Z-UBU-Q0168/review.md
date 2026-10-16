# Model-Committee Review

Run: `20261016T023445Z-UBU-Q0168`  
Question: `UBU-Q0168`  
Base commit: `206a876869bf01054ced1f325e38d0ff014af5cb`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0168-stream-presentation-frontier`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 95.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0168-stream-presentation-frontier` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0168-stream-presentation-frontier` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0168-stream-presentation-frontier` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0168-stream-presentation-frontier` | `openai` | `codex` | yes | 95 | Mechanical validation confirms clean application, allowlisted files and preserved question schema. The proposal directly strengthens streamed degradation behavior through observation ordering, presentation-based coverage accounting and targeted race fixtures. It preserves existing qualification gates and correctly leaves UBU-Q0168 Open without claiming measured results. | None | Rejecting incomparable multi-source updates can reduce live coverage; frozen deployment trials must establish acceptable usefulness., Controller enforcement and numerical acceptance profiles remain unverified, as explicitly acknowledged. |
| `ubu-q0168-stream-presentation-frontier` | `openai` | `claude` | no | n/a | claude exited with 1 | None | None |

## Selected Summary

Prevent still-fresh out-of-order stream updates from replacing newer guidance or inflating live-guidance coverage.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Controller enforcement and numerical acceptance profiles remain unverified, as explicitly acknowledged.
- Rejecting incomparable multi-source updates can reduce live coverage; frozen deployment trials must establish acceptable usefulness.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261016T023445Z-UBU-Q0168/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261016T023445Z-UBU-Q0168/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261016T023445Z-UBU-Q0168"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
