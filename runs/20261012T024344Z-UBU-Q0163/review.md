# Model-Committee Review

Run: `20261012T024344Z-UBU-Q0163`  
Question: `UBU-Q0163`  
Base commit: `934e7fd2f74b06bc847942882ef01fde40938de4`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0163-openai-gpt-6-retirement-gates`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0163-openai-gpt-6-retirement-gates` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0163-openai-gpt-6-retirement-gates` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0163-openai-gpt-6-retirement-gates` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0163-openai-gpt-6-retirement-gates` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0163-openai-gpt-6-retirement-gates` | `openai` | `codex` | yes | 94 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch directly specifies auditable capture, occupancy, duplicate prevention and handoff gates, correctly recognizes existing expanded-instance capture, and retains residual protection for incomplete coverage. Keeping UBU-Q0163 Open avoids claiming verified retirement. No blocking defect is evident in the proposed specification. | None | The source/path inventory, passing implementation checks and operator-approved handoff remain outstanding; the specification alone cannot justify removing a blocker. |
| `UBU-Q0163-openai-gpt-6-retirement-gates` | `openai` | `claude` | yes | 80 | Patch applies cleanly, touches only the three allowlisted documents, and leaves the UBU-Q0163 metadata line and Status: Open untouched, which is appropriate for a Human-approval-required process question with no implementation evidence yet. It answers the question as posed by defining concrete verification gates (one Task lineage per occurrence, UTC/DST correctness, backend agreement on busy/free capacity, no double counting, residual blocking for unsupported inputs, operator-approved batch handoff with plan invalidation) and correctly refuses to claim retirement has happened. It honours the existing direction that timed expanded instances already capture. Deductions are for density, unverifiable cross-references, and minor redundancy rather than for any correctness or schema failure. | Confirm UBU-D0279 and the other cross-referenced terms exist in DESIGN.md/DECISIONS.md at the base commit before merge., Confirm UBU-D0318 is the next unallocated decision id., Optionally trim the DESIGN.md insert or move manifest details into the decision so §1 stays readable. | DESIGN.md text cites UBU-D0279, a 'CPU reference path', enabled planner backends and Compartment policy; if any of those names do not match existing design vocabulary at the base commit, the new section will dangle., UBU-D0318 is marked Accepted while UBU-Q0163 stays Open with Resolved by: None; the prose explains the split, but downstream tooling that pairs decisions with question status may read it as inconsistent., The new DESIGN.md §1 block is four dense paragraphs introducing a 'coverage manifest' without saying where it is stored or what format it takes, which makes the gate harder to audit later., Decision number UBU-D0318 is allocated by the proposal without visible evidence that it is the next free number; a concurrent proposal could collide., The Current direction addition partly duplicates the Resolution paragraph, adding redundancy to the question record. |

## Selected Summary

Define verified capture, occupancy and handoff gates for retiring recurring-busy capacity blockers; keep UBU-Q0163 open pending implementation evidence.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- DESIGN.md text cites UBU-D0279, a 'CPU reference path', enabled planner backends and Compartment policy; if any of those names do not match existing design vocabulary at the base commit, the new section will dangle.
- Decision number UBU-D0318 is allocated by the proposal without visible evidence that it is the next free number; a concurrent proposal could collide.
- The Current direction addition partly duplicates the Resolution paragraph, adding redundancy to the question record.
- The new DESIGN.md §1 block is four dense paragraphs introducing a 'coverage manifest' without saying where it is stored or what format it takes, which makes the gate harder to audit later.
- The source/path inventory, passing implementation checks and operator-approved handoff remain outstanding; the specification alone cannot justify removing a blocker.
- UBU-D0318 is marked Accepted while UBU-Q0163 stays Open with Resolved by: None; the prose explains the split, but downstream tooling that pairs decisions with question status may read it as inconsistent.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T024344Z-UBU-Q0163/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T024344Z-UBU-Q0163/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T024344Z-UBU-Q0163"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
