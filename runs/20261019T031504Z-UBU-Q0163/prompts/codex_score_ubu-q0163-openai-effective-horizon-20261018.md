# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0163`  
Question title: `Retire the uncaptured recurring-busy capacity workaround`  
Base commit: `1f937d084092f0c853e0a8644c4717dbb500eec5`

```markdown
## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-18 Scored from commit: None

### Question

What verified capture/occupancy behavior allows removal of the temporary manual blocking Static event used for uncaptured recurring busy time during the Phase 1b-to-Phase 2 transition?

### Current direction

`UBU-D0318` specifies the scoped capture, capacity and blocker-handoff gates in DESIGN.md §1. The source audit below inventories the expanded-instance implementation paths and identifies a cancellation/retirement gap. The next bounded implementation work is the proposed cancellation remediation ticket, Google read-path qualification and wire-to-planner acceptance slice below. A separate bounded series-truncation follow-up below addresses changed recurrence rules without assuming per-instance cancellation delivery. The cancellation ticket also needs the payload-minimization/replay extension below because source identity is currently reconstructed from Task payload. The mixed-blocker handoff ticket below supplies a separate bounded replacement and residual-capacity slice. Remaining work is the actual operator source/series inventory, confirmation of deployed paths/revisions, revision-pinned passing checks and operator approval for the identified blocker replacements. Unsupported or incomplete coverage retains residual blocking.

Recorded operator-reported limitation: uncaptured busy time is invisible to the planner, so a manual capacity blocker can prevent placement there but can also collide with subsequently captured commitments. Timed expanded recurrence instances already capture as individual Tasks; do not describe all recurrence as unsupported. Series semantics, unsupported inputs and missed/on-demand capture still need review. Remove the workaround only when relevant recurring commitments demonstrably occupy the correct capacity, without duplicates or double counting. No operator event or collision tally is reproduced here.

### Revision-pinned source audit and implementation handoff

A 2026-10-12 read-only source audit of the clean local `ubu-phase2/ubu-orchestrator` checkout at `9fddef16990ac89d07a168c75bf04b77c35b9bc3` identifies these paths. This is static source evidence; the referenced test sources were inspected, not executed, and the operator's actual source/series inventory remains outstanding.

- Listing: `src/services/calendar_wire.rs::list_request` requests `singleEvents=true` within the requested time range, and `src/services/calendar_google.rs::GoogleCalendarApi::list_events` follows pagination with a repeated-token guard. `calendar_wire::parse_event_with_diagnostic` requires concrete RFC3339 `dateTime` endpoints and explicitly skips all-day inputs. Listing completion alone does not establish usable coverage of skipped inputs.
- Occurrence lineage: `src/services/calendar_capture.rs::{plan_capture,capture}` makes unownable timed event ids, including expanded recurrence-instance ids, Static Tasks at their validated windows, with capacity controlled by transparency. `src/services/calendar_sources.rs::by_source` reconstructs the persisted `provenance.source.source_id` mapping used to reuse Task handles on recapture. These paths record individual events rather than reconstructing a series; restart/replay correctness still needs behavioral evidence.
- Occupancy and projection: `src/services/planning_service.rs::build_request_from_store_with_context` includes capacity-bearing Static Tasks as anchored TaskSpecs and separates non-capacity Static Tasks. `src/services/calendar_projection.rs::{external_id_for,desired_events}` excludes captured origins whose ids cannot round-trip. These source paths identify the intended planner/export boundaries; they do not establish CPU/backend parity or a safe blocker handoff.
- Cancellation/absence gap: `calendar_wire::parse_event_with_diagnostic` skips entries with `status: cancelled`; `calendar_capture::capture` processes observed candidates without retiring absent captured Tasks. `src/services/calendar_reconciliation_service.rs::reconcile` emits `capture_source_removed` for an active captured Task absent within its observed horizon and retains the Task for operator review. That diagnostic is not an admitted cancellation or reservation retirement. The audited paths therefore do not demonstrate removal of stale future occupancy after an authoritative cancellation or series edit; absence from an incomplete response must continue to preserve protection.

Existing assertion sources are `tests/calendar_occupancy.rs` for repeat capture, moves, projection exclusion and a Dynamic-placement control, and `tests/calendar_capture.rs::vanished_source_is_reported_by_reconcile_without_deleting_task` for retained missing-source Tasks. Their presence is not a passing result for `UBU-D0318`.

The implementation handoff is a synthetic expanded-instance capture, restart, explicit cancellation and recapture/reconciliation reproduction, with incomplete-list and out-of-horizon controls. Locate or implement the policy-authorized ordinary-admission retirement/supersession path for cancellations and series edits used by the reviewed scope; verify that it removes only stale future reservations, preserves history and invalidates affected Plans while unproven cancellation retains protection. Attach revision-pinned commands, backend/configuration identities and results for that path and the remaining `UBU-D0318` gates to the actual scoped coverage manifest before reviewing the blocker replacement batch. The proposed ticket below narrows the explicit-cancellation repair; series-edit coverage remains a separate retirement gate. No filed repair ticket or passing implementation result is recorded here.

### Proposed cancellation implementation remediation

Proposed ticket: **Carry explicit occurrence cancellations from the wire reader through durable admission** in `ubu-orchestrator`, linked to `UBU-Q0163`. File it with the affected paths, implementation revision and a raw-wire reproduction. Its scope is explicit cancellation of an already captured future occurrence; it does not establish series-edit support or authorize blocker retirement.

A 2026-10-14 follow-up static inspection of the same clean `9fddef16990ac89d07a168c75bf04b77c35b9bc3` checkout identifies the missing transport and binding seams: `src/services/calendar_client.rs::CalendarApi::list_events` and `calendar_google::GoogleCalendarApi::list_events` return only `Vec<DesiredEvent>`; `calendar_projection::DesiredEvent` carries neither account/calendar scope nor source revision/observation ordering. `calendar_sources::by_source` indexes captured Tasks by the bare persisted `source_id`. These inspected representations do not supply the slice's required source-scope and ordering bindings. Adding cancellation parsing alone would therefore leave admission and replay safety unverified. This inspection executed no behavioral checks.

A 2026-10-15 static follow-up at the same clean revision identifies a lifecycle dependency: `calendar_sources::by_source` selects Tasks by `payload_json` source provenance and reads `source_id` from that payload; `origins_by_task` derives its map from those rows. `calendar_capture::capture` consults that map before skipping inactive Tasks, and `calendar_apply::preview` uses the derived origins to suppress source deletions. These inspected guards therefore depend on retained payload provenance. They do not establish replay or source-deletion protection after `UBU-D0259` payload minimization; no purge/replay checks were executed.

- **Read boundary:** extend `calendar_wire`, `calendar_client` and `calendar_google` to carry identity-bound cancellation observations separately from timed live events and parsing diagnostics, including cancellation entries without live-event timestamps. Exercise the configured production read path's cancellation delivery and pagination; unavailable delivery leaves that scope unverified. Omission, unsupported input and `capture_source_removed` remain non-cancellation outcomes.
- **Durable binding:** bind both live and cancellation observations to the permitted source account/calendar, stable occurrence identity, verifiable source ordering and admitted Task/version. Persist enough permitted lineage and ordering metadata to reject stale replay after restart and retirement. Establish legacy bindings from trusted evidence before using this path; an unbound or ambiguous legacy `source_id` retains protection and review. Neither matching times nor a matching bare event id in another account/calendar establishes authority.
- **Ordinary admission:** use the existing mutation-envelope and lifecycle paths under `UBU-D0258`/`UBU-D0259`; do not hard-delete the Task, set successful completion or rewrite Logs. Make retirement, admission/idempotency outcome and required replay metadata crash-consistent. Apply affected Plan invalidation and prevent certification, reuse or projection against the retired reservation until recalculation is certified. Failed authority, policy, ordering or version checks release no reservation.
- **Capture and projection integration:** make recapture consult the admitted retirement and source-order bindings before updating or creating a Task. Retry, restart and older live replay cannot restore the reservation or create a replacement Task. A provably newer source change requires separate current admission. Preserve policy-permitted source lineage needed by `calendar_sources` and `calendar_apply` to distinguish source commitments from UbU-created projections; retiring a captured Task must not enqueue deletion of its external source.
- **Payload minimization:** keep the permitted source-origin classification and scope/occurrence/order bindings needed by these guards in durable admitted lifecycle/replay structure when active payload is purged. Retain no source content or identifiers forbidden by current policy, and never reconstruct purged fields from cached observations. If necessary bindings cannot be retained or verified, invalidate the affected coverage and withhold affected automatic recapture/projection pending permitted requalification; missing provenance must not turn an old occurrence into a new Task.

Extend the bounded slice with identical occurrence ids in two distinct source accounts/calendars, an unbound legacy captured Task, and missing or incomparable source-order evidence. In each negative control, the targeted reservation remains protected and unrelated Tasks are unchanged. Exercise production raw-wire parsing and durable admission rather than injecting only normalized live events. Attach the implementation revision, exact commands, fixture digests and outcomes to the filed ticket; its acceptance requires every row of the cancellation slice below, including crash/replay, CPU/backend occupancy and projection assertions. These are implementation requirements, not passing results.

### Google read-path qualification for the cancellation ticket

A further 2026-10-14 static inspection of the same clean `9fddef16990ac89d07a168c75bf04b77c35b9bc3` checkout confirms that `calendar_wire::list_request` sends `singleEvents=true`, `timeMin`, `timeMax` and optional `pageToken`, but no `showDeleted`, `syncToken` or `updatedMin`. `calendar_google::GoogleCalendarApi::list_events` consumes `nextPageToken` but does not retain `nextSyncToken`. `tests/calendar_wire.rs::urls_encode_both_ids_and_requests_choose_method_and_headers` asserts that bounded request shape. These are inspected sources, not executed delivery checks.

Google's [events.list reference](https://developers.google.com/workspace/calendar/api/v3/reference/events/list), checked on 2026-10-14, defaults `showDeleted` to false; with `singleEvents=true` and `showDeleted=true`, deleted instances are returned but recurring parents are excluded. The [event resource reference](https://developers.google.com/workspace/calendar/api/v3/reference/events) guarantees only `id`, `recurringEventId` and `originalStartTime` for cancelled recurring exceptions; live timestamps, `updated` and `etag` cannot be assumed present. This identifies a delivery and ordering prerequisite for the existing ticket, without establishing series-edit coverage.

- **Bounded delivery:** exercise explicit `showDeleted=true` on every page of the expanded bounded read through the configured production adapter. Record the permitted account/calendar, request parameters and complete page chain. Demonstrate a sparse cancelled exception on a later page reaching the separate cancellation channel. If that read cannot deliver the reviewed cancellations, retain protection and qualify another authorized read path before claiming coverage.
- **Sparse evidence and ordering:** bind the exception to persisted occurrence lineage using its scoped identity and original occurrence reference, including after a move. Establish verifiable ordering even when modification fields are absent; receipt time, a new mutation envelope or lexical comparison of opaque tokens cannot establish a newer source observation. A live read started before cancellation and delivered afterwards must not restore retired occupancy. Unproved ordering retains protection under the existing ticket's binding gate.
- **Optional cursor path:** if incremental sync is selected, Google's [sync guide](https://developers.google.com/workspace/calendar/api/guides/sync) and list reference require compatible query parameters; `syncToken` cannot accompany `timeMin` or `timeMax`. Bind the cursor to the permitted source and read configuration, and advance its durable checkpoint only after the full page chain and each observation's durable admission/rejection outcome. On HTTP 410, reset only provider cursor/cache state and rebaseline; preserve canonical Tasks, tombstones, Logs and replay metadata. Invalidate affected coverage and retain unresolved reservations until verification completes. Cursor reset is not cancellation admission.

### Bounded cancellation/retirement acceptance slice

Use a disposable synthetic calendar and durable store with the clock fixed at `2026-10-19T09:00:00Z` and planning horizon `[09:00, 15:00)` UTC that day. Seed two opaque expanded instances with distinct source occurrence ids: A at `[10:00, 11:00)` and B at `[13:00, 14:00)`, plus a previously logged occurrence outside the horizon. Record the original Task/source lineage and historical records. Exercise the production wire-list parsing, capture, store reopen, ordinary admission, planning-request and projection paths; a mock supplying only already-parsed live events cannot test the cancellation gap. Isolate this slice from the workaround blocker so residual protection cannot mask stale captured occupancy.

| Check | Required result |
| --- | --- |
| Capture A/B, repeat capture, reopen the same store and recapture | One Task lineage per occurrence; exactly A/B occupy their original windows, with no duplicate capacity. |
| Deliver an explicit cancellation for A through the wire adapter, including a case without live-event `dateTime` fields | Identity-bound cancellation evidence reaches ordinary admission; after certified recalculation only B occupies capacity. A's former interval is free, historical records remain unchanged, and no external source deletion is issued. |
| Repeat the cancellation, restart, then replay an older live observation of A or finish a live read started before cancellation | No repeated retirement effect, duplicate Task or restored reservation; B remains unchanged. Later receipt or a fresh local envelope cannot make stale source evidence newer. A newer source change requires its own current admission checks. |
| After admitted cancellation of A, apply policy-authorized active-payload purge, discard rebuildable caches, reopen the store and replay the older live A and cancellation | A's permitted tombstone structure and replay outcome survive without active payload; no replacement Task, restored occupancy or external source deletion appears. B and permitted historical records remain unchanged. Exercise retained-binding and policy-withheld-binding controls: the latter leaves coverage uncertified and withholds affected automatic operations until permitted requalification. |
| Inspect every outgoing bounded-list page and deliver a sparse cancellation for A on a later page | Explicit deletion delivery is requested consistently; the production adapter preserves scoped occurrence evidence without requiring summary, live start/end, updated or etag. A retires only with independently verified ordering and ordinary admission; otherwise its reservation stays protected. |
| If cursor mode is used, interrupt pagination/checkpoint persistence, reopen/retry, then exercise HTTP 410 and rebaseline | No cancellation is lost through cursor advancement, no duplicate retirement occurs, and canonical state/history/replay metadata survive reset. Incomplete or reset coverage remains uncertified; unresolved reservations stay protected. |
| Interrupt cancellation admission before and after its durable commit, then reopen and retry | Either the original reservation remains or one durable retirement survives with affected Plans invalidated; no intermediate state can be certified or projected. |
| From the original baseline, omit A from an otherwise complete bounded list, truncate pagination, narrow observation outside A's interval, or skip an unsupported input | A/B retain protection; absence or a parsing diagnostic admits no cancellation and cannot certify the affected coverage. |
| From the original baseline, supply cancellation evidence with an unknown occurrence, stale/conflicting source or Task bindings, or denied authority/policy | No reservation is released; retain protection and apply ordinary rejection/review rules. |

The adapter must bind cancellation evidence to the source account/calendar, stable occurrence identity, source revision or verifiable observation ordering, and current admitted Task version. Carry it separately from timed live-event validation; do not turn a skipped `cancelled` entry or `capture_source_removed` diagnostic into an inferred deletion. If identity, ordering or authority cannot be established, the slice remains unverified. Retirement changes the future reservation through ordinary admission; it must not fabricate successful completion, rewrite Logs, or authorize cancellation of the external commitment.

For every row, attach the implementation revision, exact command and fixture digest, backend/configuration identity, admission outcome, Task/version lineage, occupied/free interval assertions and affected Plan invalidation/recalculation result. Check the CPU reference path and each enabled planner backend with 30-minute Dynamic probes restricted to A's or B's window: neither fits at baseline; after admitted cancellation, demonstrate a certified feasible placement inside A and no feasible placement inside B. Attach the results and any remaining production-path gaps to the cancellation remediation ticket above. These are required results, not executed checks. Passing this slice supplies cancellation evidence only; series-edit, timezone/DST, unsupported-input, mixed-blocker and reviewed-handoff gates under `UBU-D0318` still apply before any workaround retirement.

### Bounded series-truncation follow-up

Proposed ticket: **Qualify and admit recurrence-rule truncation for captured future instances** in `ubu-orchestrator`, linked to `UBU-Q0163`. The explicit-cancellation ticket does not establish this case.

A 2026-10-15 read-only inspection of the clean `9fddef16990ac89d07a168c75bf04b77c35b9bc3` checkout confirms that `src/services/calendar_wire.rs::parse_event_with_diagnostic` and `calendar_projection::DesiredEvent` do not preserve `recurringEventId`, `originalStartTime` or parent recurrence rules; `calendar_capture::capture` persists the bare event `source_id` without those series bindings. Google's [recurring-events guide](https://developers.google.com/workspace/calendar/api/guides/recurringevents), checked on 2026-10-15, distinguishes original occurrence time from a moved start and states that expanded listing omits parent recurring events. The inspected bounded reader therefore supplies no parent-rule evidence for this follow-up. This is static evidence, not an executed reproduction.

- **Read qualification:** preserve permitted scoped series/occurrence lineage through the production reader and durable store. Qualify an authorized read supplying an ordered, authoritative rule change and sufficient exception evidence, or equivalent provider evidence identifying the removed occurrences. A complete expanded list alone cannot establish removal, and per-instance cancellation entries must not be assumed. Missing identity, ordering or exception coverage retains reservations and leaves the affected scope uncertified; full series-level import remains optional.
- **Bounded reproduction:** use the cancellation slice's clock, durable store and horizon, with A/B belonging to one synthetic series. Capture, reopen, then shorten that series so A remains at `[10:00, 11:00)` and B at `[13:00, 14:00)` is removed; supply no explicit per-instance cancellation. Through the qualified production read and ordinary admission, retire only B's stale future reservation. Preserve A's Task/version and logged history, issue no external source deletion, invalidate affected Plans and certify recalculation. Reuse the cancellation ticket's authority, lifecycle, crash-consistency and replay gates.
- **Controls:** from baseline, deliver only a complete expanded list omitting B, a truncated response, an out-of-scope or stale parent observation, or missing/incomparable ordering or exception evidence. None releases B or certifies affected coverage. After a valid retirement, restart and replay the older rule and expanded B; neither restores occupancy nor creates another Task. Interrupt admission on both sides of durable commit and verify the original reservation or one durable retirement survives with no certifiable intermediate state.
- **Evidence:** attach before/after raw-wire fixture digests, source and Task/version bindings, implementation revision, exact commands, backend/configuration identities and admission/Plan outcomes to the filed ticket and scoped manifest. On CPU and every enabled planner backend, 30-minute probes fit neither A nor B at baseline; after admitted truncation they fit B's former interval and cannot fit A. Report unsupported production paths explicitly. Passing this slice covers only the exercised truncation semantics; other series edits and the remaining `UBU-D0318` retirement gates still require evidence and operator approval. No ticket filing or passing result is recorded here.

### Bounded mixed-blocker handoff

Proposed ticket: **Implement and verify a version-bound mixed-blocker replacement** in `ubu-orchestrator`, linked to `UBU-Q0163`. This exercises the existing `UBU-D0318` handoff gate independently of cancellation and series-edit remediation; it supplies no capture qualification or approval to retire the operator's workaround.

Use the cancellation slice's fixed clock, horizon and disposable durable store. In a single-capacity fixture, seed workaround Static Task W at `[10:00, 14:00)` UTC. Supply opaque expanded occurrences A at `[10:00, 11:00)` and B at `[13:00, 14:00)` through the production wire/capture path. The synthetic manifest explicitly associates W with A/B's replacement coverage and an unverified interval `[11:00, 13:00)`; matching times or titles cannot establish that association. Record A/B's admitted Task/version lineage. Prevent certification, reuse and projection of intermediate doubled occupancy while capturing replacements and changing W, including when A/B were captured before handoff review.

The reviewed change retires W through ordinary tombstone admission and creates one residual capacity-bearing Static Task R at `[11:00, 13:00)`, preserving A/B's admitted Tasks and external commitments. Bind approval to the manifest, exact W-to-R replacement, A/B lineage, current object/policy versions and horizon. Apply the batch or protected serialized equivalent under `UBU-D0258`/`UBU-D0259`; preserve history and restrict any projection cleanup to authorized workaround changes.

| Check | Required result |
| --- | --- |
| Preview without approval; separately approve a preview, change a bound source, Task, policy version or horizon, then execute the stale approval | Preview releases no reservation. Missing approval or stale bindings refuse the handoff; W retains protection and changed coverage requires fresh verification/review. Intermediate captured duplicates cannot become a certified or projected Plan. |
| Approve the current manifest and execute the handoff | W is retired, exactly one R survives, and A/B lineage is unchanged. Inspect planner capacity inputs, not only their occupied union: exactly one reservation covers each instant of `[10:00, 14:00)` and none covers `[09:00, 10:00)` or `[14:00, 15:00)`. Captured origins and their projections add no duplicate reservation. |
| Interrupt before/after each durable handoff boundary, reopen the store and retry/replay the same approved operation | Protection survives every interruption. Recovery yields the original protected state or one completed W-to-R handoff; any pending serialized state remains uncertifiable and unprojectable. No duplicate R, repeated retirement effect or lost residual interval appears. Affected old Plans remain invalid until recalculation is certified. |
| Add distinct source occurrence C at `[10:30, 11:00)`, with its own lineage, in a separate control | C remains distinct from A and W's replacement mapping. The A/C overlap remains a genuine fixed-commitment conflict; neither time-based deduplication nor blocker replacement may erase it or certify it as conflict-free. |

On the CPU reference path and each enabled planner backend, use separate 30-minute Dynamic probes restricted to `[10:15, 10:45)`, `[11:30, 12:00)` and `[13:15, 13:45)`: none fits after handoff. Controls restricted to `[09:15, 09:45)` and `[14:15, 14:45)` must have certified feasible placements. These probes alone cannot detect double counting; retain the capacity-input multiplicity assertions and the separate genuine-overlap control.

Attach implementation revision, exact commands, fixture/manifest digests, backend/configuration identity, approval/admission outcomes, Task/version lineage, crash/replay results, capacity assertions and Plan invalidation/recalculation evidence to the filed ticket and scoped manifest. Unobserved paths remain unverified. No filing, implementation result or actual blocker retirement is recorded here; cancellation, series edits and the other `UBU-D0318` gates remain outstanding.

### Resolution

Open. `UBU-D0318` remains the retirement specification. The source audits, proposed cancellation remediation ticket with its payload-minimization/replay extension, Google read-path qualification, bounded cancellation slice, separate series-truncation follow-up and mixed-blocker handoff ticket identify implementation work and required evidence; none supplies passing results. Filing and implementing the repair, qualified cancellation delivery/ordering and any cursor recovery, actual scoped source/series coverage, confirmation of deployed paths/revisions, the remaining required checks and the operator-approved blocker handoff remain outstanding. This documentation change removes no blocker.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0163-openai-effective-horizon-20261018",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0163",
    "base_commit": "1f937d084092f0c853e0a8644c4717dbb500eec5",
    "summary": "Add a source-pinned implementation ticket and acceptance slice for capture coverage when the effective planning horizon expands.",
    "rationale": "Capture and explicit planning resolve their windows separately. The proposed slice verifies that extending a planning window cannot treat uncaptured busy time as free. It advances an existing UBU-D0318 gate while preserving the question's Open status and remaining operator-review requirements.",
    "changed_files": [
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex 07a6293..0000000 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3135,21 +3135,21 @@ Open. Append-only correction is already decided. Static source audits identify a\n ## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-18 Scored from commit: None\n \n ### Question\n \n What verified capture/occupancy behavior allows removal of the temporary manual blocking Static event used for uncaptured recurring busy time during the Phase 1b-to-Phase 2 transition?\n \n ### Current direction\n \n-`UBU-D0318` specifies the scoped capture, capacity and blocker-handoff gates in DESIGN.md \u00a71. The source audit below inventories the expanded-instance implementation paths and identifies a cancellation/retirement gap. The next bounded implementation work is the proposed cancellation remediation ticket, Google read-path qualification and wire-to-planner acceptance slice below. A separate bounded series-truncation follow-up below addresses changed recurrence rules without assuming per-instance cancellation delivery. The cancellation ticket also needs the payload-minimization/replay extension below because source identity is currently reconstructed from Task payload. The mixed-blocker handoff ticket below supplies a separate bounded replacement and residual-capacity slice. Remaining work is the actual operator source/series inventory, confirmation of deployed paths/revisions, revision-pinned passing checks and operator approval for the identified blocker replacements. Unsupported or incomplete coverage retains residual blocking.\n+`UBU-D0318` specifies the scoped capture, capacity and blocker-handoff gates in DESIGN.md \u00a71. The source audit below inventories the expanded-instance implementation paths and identifies a cancellation/retirement gap. The next bounded implementation work is the proposed cancellation remediation ticket, Google read-path qualification and wire-to-planner acceptance slice below. A separate bounded series-truncation follow-up below addresses changed recurrence rules without assuming per-instance cancellation delivery. The cancellation ticket also needs the payload-minimization/replay extension below because source identity is currently reconstructed from Task payload. The mixed-blocker handoff ticket below supplies a separate bounded replacement and residual-capacity slice. The effective-horizon coverage follow-up below addresses capture and planning requests that resolve different windows. Remaining work is the actual operator source/series inventory, confirmation of deployed paths/revisions, revision-pinned passing checks and operator approval for the identified blocker replacements. Unsupported or incomplete coverage retains residual blocking.\n \n Recorded operator-reported limitation: uncaptured busy time is invisible to the planner, so a manual capacity blocker can prevent placement there but can also collide with subsequently captured commitments. Timed expanded recurrence instances already capture as individual Tasks; do not describe all recurrence as unsupported. Series semantics, unsupported inputs and missed/on-demand capture still need review. Remove the workaround only when relevant recurring commitments demonstrably occupy the correct capacity, without duplicates or double counting. No operator event or collision tally is reproduced here.\n \n ### Revision-pinned source audit and implementation handoff\n \n A 2026-10-12 read-only source audit of the clean local `ubu-phase2/ubu-orchestrator` checkout at `9fddef16990ac89d07a168c75bf04b77c35b9bc3` identifies these paths. This is static source evidence; the referenced test sources were inspected, not executed, and the operator's actual source/series inventory remains outstanding.\n \n - Listing: `src/services/calendar_wire.rs::list_request` requests `singleEvents=true` within the requested time range, and `src/services/calendar_google.rs::GoogleCalendarApi::list_events` follows pagination with a repeated-token guard. `calendar_wire::parse_event_with_diagnostic` requires concrete RFC3339 `dateTime` endpoints and explicitly skips all-day inputs. Listing completion alone does not establish usable coverage of skipped inputs.\n - Occurrence lineage: `src/services/calendar_capture.rs::{plan_capture,capture}` makes unownable timed event ids, including expanded recurrence-instance ids, Static Tasks at their validated windows, with capacity controlled by transparency. `src/services/calendar_sources.rs::by_source` reconstructs the persisted `provenance.source.source_id` mapping used to reuse Task handles on recapture. These paths record individual events rather than reconstructing a series; restart/replay correctness still needs behavioral evidence.\n - Occupancy and projection: `src/services/planning_service.rs::build_request_from_store_with_context` includes capacity-bearing Static Tasks as anchored TaskSpecs and separates non-capacity Static Tasks. `src/services/calendar_projection.rs::{external_id_for,desired_events}` excludes captured origins whose ids cannot round-trip. These source paths identify the intended planner/export boundaries; they do not establish CPU/backend parity or a safe blocker handoff.\n@@ -3235,9 +3235,28 @@ On the CPU reference path and each enabled planner backend, use separate 30-minu\n \n Attach implementation revision, exact commands, fixture/manifest digests, backend/configuration identity, approval/admission outcomes, Task/version lineage, crash/replay results, capacity assertions and Plan invalidation/recalculation evidence to the filed ticket and scoped manifest. Unobserved paths remain unverified. No filing, implementation result or actual blocker retirement is recorded here; cancellation, series edits and the other `UBU-D0318` gates remain outstanding.\n \n+### Bounded effective-horizon coverage follow-up\n+\n+Proposed ticket: **Bind recurring-busy coverage to the effective planning window** in `ubu-orchestrator`, linked to `UBU-Q0163`. This independently exercises `UBU-D0318`'s horizon-advancement gate; it requires no cancellation or series-edit implementation.\n+\n+A 2026-10-18 read-only inspection of the clean `9fddef16990ac89d07a168c75bf04b77c35b9bc3` checkout identifies separate scope resolution: `src/services/calendar_range.rs::CalendarTimeRange::planning` calls `planning_service::resolve_time_window` with no explicit horizon, and `calendar_capture::capture` filters new foreign capture against that range. `planning_service::generate` separately passes `GeneratePlanningRequest.horizon` to `build_request_from_store_with_context`. The builder can also widen the kernel `time_window` for participating capacity-bearing Static Tasks. These inspected paths do not demonstrate that prior capture covers the effective planning window. No behavioral check was executed.\n+\n+Pass the intended read scope explicitly through the authorized capture path, or withhold/protect capacity outside verified coverage. Bind the coverage check to the final effective planning window, permitted source scope, completeness evidence and current source/Task/policy versions before certification, reuse or projection. A changed window cannot inherit a smaller window's coverage claim or handoff approval. Further window changes require another check within finite configured read/work limits; exhaustion retains protection. Successful listing alone cannot qualify skipped or policy-withheld inputs, and this check grants no additional read authority.\n+\n+Use the cancellation slice's fixed clock and a disposable durable store with no saved Calendar window and a six-hour default horizon. Supply two distinct opaque expanded occurrences through the production wire adapter: A at `[10:00, 11:00)` and B at `[16:00, 17:00)` UTC on `2026-10-19`. Capture the default window H0 `[09:00, 15:00)`, then request explicit planning window H1 `[09:00, 18:00)`. Record the outgoing read bounds, foreign-capture range and actual kernel window; seed only a synthetic H0 coverage manifest.\n+\n+| Check | Required result |\n+| --- | --- |\n+| Request H1 with only H0 capture/coverage evidence | B's absence from H0 cannot establish free capacity in H1. Withhold affected placements/certification/reuse/projection or retain residual blocking until H1 is qualified; preserve A's lineage and history. |\n+| Complete an authorized H1 read/capture, repeat it and reopen the store | Exactly one Task per A/B occurrence, with unchanged A lineage and no source deletion. With workaround/residual blockers excluded from these occupancy assertions, separate 30-minute Dynamic probes restricted to `[16:15, 16:45)` cannot fit, while controls restricted to `[17:15, 17:45)` have certified feasible placements. |\n+| Truncate the added-window read, skip an unsupported busy input there, or deny its source access | The affected extension remains unverified and protected/withheld; successful H0 evidence and partial admissions cannot certify it. Retry preserves admitted lineage without duplicates. |\n+| Restart with H0 evidence, replay a stale H0 manifest/approval, or change the bound effective window before execution | No uncovered interval becomes certified free and no stale handoff executes. Recheck current bindings; invalidate affected old Plans and require certified recalculation before reuse/projection. |\n+\n+Attach implementation revision, exact commands, fixture/manifest digests, requested/read/kernel windows, backend/configuration identities, Task/version lineage, admission outcomes and Plan results to the filed ticket and scoped manifest. Run the occupied/free assertions on the CPU reference path and every enabled planner backend. This proposed slice supplies no passing result, filed ticket or actual blocker retirement; the operator inventory and remaining `UBU-D0318` gates still apply.\n+\n ### Resolution\n \n-Open. `UBU-D0318` remains the retirement specification. The source audits, proposed cancellation remediation ticket with its payload-minimization/replay extension, Google read-path qualification, bounded cancellation slice, separate series-truncation follow-up and mixed-blocker handoff ticket identify implementation work and required evidence; none supplies passing results. Filing and implementing the repair, qualified cancellation delivery/ordering and any cursor recovery, actual scoped source/series coverage, confirmation of deployed paths/revisions, the remaining required checks and the operator-approved blocker handoff remain outstanding. This documentation change removes no blocker.\n+Open. `UBU-D0318` remains the retirement specification. The source audits, proposed cancellation remediation ticket with its payload-minimization/replay extension, Google read-path qualification, bounded cancellation slice, separate series-truncation follow-up, mixed-blocker handoff ticket and effective-horizon coverage ticket identify implementation work and required evidence; none supplies passing results. Filing and implementing the repair, qualified cancellation delivery/ordering and any cursor recovery, actual scoped source/series coverage, confirmation of deployed paths/revisions, the remaining required checks and the operator-approved blocker handoff remain outstanding. This documentation change removes no blocker.\n \n ---\n \n",
    "commit_message": "docs: add effective-horizon coverage slice for UBU-Q0163",
    "validation_notes": [
      "Generated the diff from unmodified working-tree content and proposed contents held in memory.",
      "git apply --check --whitespace=error - passed against the current working tree.",
      "Only UBU-Q0163 content changes; its single-line metadata and all other question blocks remain unchanged.",
      "Confirmed the audited orchestrator checkout is clean at 9fddef16990ac89d07a168c75bf04b77c35b9bc3.",
      "No implementation acceptance tests were executed, tickets filed or blockers retired. Repository files remain unchanged."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0163-openai-effective-horizon-20261018",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "OPEN_QUESTIONS.md"
    ],
    "question_schema_passed": true,
    "error": null,
    "normalized_patch": null,
    "warnings": [],
    "ordinary_error": null,
    "recount_error": null,
    "normalization_error": null
  }
]
```

## Provider weights

Provider weights are historical diagnostic context only in v0.2. Do not use
self-trust or author identity as score evidence.

```json
{}
```

## JSON Schema

Your output must satisfy this schema:

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": [
    "scores",
    "selected_proposal_id",
    "selection_rationale"
  ],
  "properties": {
    "scores": {
      "type": "array",
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": [
          "proposal_id",
          "score",
          "patch_applies",
          "implements_selected_work",
          "preserves_question_schema",
          "avoids_unnecessary_scope",
          "decomposition_quality",
          "risks",
          "required_fixes",
          "rationale"
        ],
        "properties": {
          "proposal_id": {
            "type": "string"
          },
          "score": {
            "type": "integer",
            "minimum": 0,
            "maximum": 100
          },
          "patch_applies": {
            "type": "boolean"
          },
          "implements_selected_work": {
            "type": "boolean"
          },
          "preserves_question_schema": {
            "type": "boolean"
          },
          "avoids_unnecessary_scope": {
            "type": "boolean"
          },
          "decomposition_quality": {
            "type": "string",
            "enum": [
              "none",
              "good",
              "bad",
              "not_applicable"
            ]
          },
          "risks": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "required_fixes": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "rationale": {
            "type": "string"
          }
        }
      }
    },
    "selected_proposal_id": {
      "type": "string"
    },
    "selection_rationale": {
      "type": "string"
    }
  }
}
```

## Scoring requirements

Score each proposal from 0 to 100.

Consider:

- whether the patch applies cleanly;
- whether it implements the selected work;
- whether it preserves the question schema;
- whether it avoids unnecessary scope;
- whether it modifies only allowed files;
- whether it creates useful decomposition if decomposition occurs;
- whether it introduces new risks;
- whether required fixes remain.

Rules:

- Score every proposal in this prompt.
- `selected_proposal_id` must refer to one scored proposal from this prompt.
- Manual override is not allowed in v0.2.
- Prefer a patch that is valid, minimal, auditable, and directly responsive.
- Do not select a proposal whose patch failed mechanical validation.
- Allowed question `Status:` values are `Archived`, `Decomposed`, `Deferred`, `Open`, `Solved`, `Superseded`; a resolved question must use `Status: Solved`.
- Reject `Status: Resolved` as a question-schema violation. Set `preserves_question_schema` to false and require `Status: Solved` as the fix; `Resolved by:` is a separate metadata field.
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
