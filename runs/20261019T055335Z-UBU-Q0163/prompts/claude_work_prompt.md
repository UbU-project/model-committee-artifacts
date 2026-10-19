# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0163`  
Question title: `Retire the uncaptured recurring-busy capacity workaround`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `eb861e919fecf938de4defc50d2e208a61610dba`

```markdown
## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-19 Scored from commit: None

### Question

What verified capture/occupancy behavior allows removal of the temporary manual blocking Static event used for uncaptured recurring busy time during the Phase 1b-to-Phase 2 transition?

### Current direction

`UBU-D0318` specifies the scoped capture, capacity and blocker-handoff gates in DESIGN.md §1. The source audit below inventories the expanded-instance implementation paths and identifies a cancellation/retirement gap. The next bounded implementation work is the proposed cancellation remediation ticket, Google read-path qualification and wire-to-planner acceptance slice below. A separate bounded series-truncation follow-up below addresses changed recurrence rules without assuming per-instance cancellation delivery. The cancellation ticket also needs the payload-minimization/replay extension below because source identity is currently reconstructed from Task payload. The mixed-blocker handoff ticket below supplies a separate bounded replacement and residual-capacity slice. The timezone, horizon-crossing and unsupported-input slice below bounds the remaining `UBU-D0318` capture-qualification gate. Remaining work is the actual operator source/series inventory, confirmation of deployed paths/revisions, revision-pinned passing checks and operator approval for the identified blocker replacements. Unsupported or incomplete coverage retains residual blocking.

The executed U3 regression below isolates a concrete repair within the existing unsupported-input ticket: a timed recurrence parent passes through the production wire parser without a diagnostic. Implement that ticket's parent-rejection requirement and exercise its capture/coverage row before claiming unsupported-input qualification.

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

### Bounded timezone, horizon-crossing and unsupported-input coverage slice

Proposed ticket: **Qualify UTC occupancy, unsupported-input outcomes and horizon-advancement rechecks for expanded-instance capture** in `ubu-orchestrator`, linked to `UBU-Q0163`. This exercises the `UBU-D0318` timezone/DST, horizon-crossing, unsupported-input, incomplete-capture and horizon-advancement gate independently of the cancellation, series-truncation and mixed-blocker tickets. It supplies no cancellation or series-edit evidence and no approval to retire the operator's workaround.

The 2026-10-12 audit above already records that `calendar_wire::parse_event_with_diagnostic` requires concrete RFC3339 `dateTime` endpoints and skips all-day inputs, and that `calendar_wire::list_request` bounds the read by `timeMin`/`timeMax`. It does not establish how delivered offsets become UTC occupancy across a DST transition, whether occurrences straddling a horizon edge occupy their in-horizon portion, whether a skipped input leaves its interval visibly uncovered in the coverage manifest, or whether a stale manifest can be reused after horizon advancement. The executed U3 parser check below establishes a narrower implementation gap; UTC occupancy, horizon-edge capture and manifest behavior remain unverified.

**Executed U3 parser regression (2026-10-19).** The clean `ubu-orchestrator` checkout at `9fddef16990ac89d07a168c75bf04b77c35b9bc3` was tested in a disposable source copy without implementation edits or dependency overrides. On Linux x86_64 with rustc 1.99.1 (`58cf8f9c3`) and cargo 1.99.1 (`5f94df478`), the existing `calendar_capture` (10), `calendar_occupancy` (10) and `calendar_wire` (14) tests all passed. The additional regression below failed: `calendar_wire::parse_event_list` returned both `q0163series_20261031T130000Z` and `q0163series` as live events, with `diagnostics: []`, rather than rejecting the parent carrying `RRULE:FREQ=DAILY;COUNT=2`. The command exited 101 at the event-count assertion (actual 2, required 1). This directly exercises production raw-wire parsing; it establishes no Google delivery, Task admission, durable replay, planner-backend or blocker-handoff result.

To reproduce, copy only `Cargo.toml`, `Cargo.lock`, `rust-toolchain.toml`, `src/`, `fixtures/` and `tests/` from that checkout to a disposable directory, save the following as `tests/q0163_unexpanded.rs`, and run `cargo test --offline --locked --test calendar_wire --test calendar_occupancy --test calendar_capture --test q0163_unexpanded -- --nocapture` there. Integration credentials and registration material are unnecessary and were not copied.

```rust
use serde_json::json;
use ubu_orchestrator::services::calendar_wire::parse_event_list;

#[test]
fn unexpanded_parent_is_rejected_without_losing_an_expanded_instance() {
    let instance = json!({
        "id":"q0163series_20261031T130000Z", "summary":"Synthetic busy",
        "start":{"dateTime":"2026-10-31T13:00:00Z"},
        "end":{"dateTime":"2026-10-31T14:00:00Z"},
        "transparency":"opaque"
    });
    let mut parent = instance.clone();
    parent["id"] = json!("q0163series");
    parent["recurrence"] = json!(["RRULE:FREQ=DAILY;COUNT=2"]);
    let raw = json!({"items":[instance,parent]});
    let (events, diagnostics) = parse_event_list(&raw);
    println!("Q0163_OBSERVED={}", json!({
        "raw":raw, "ids":events.iter().map(|e| &e.external_id).collect::<Vec<_>>(),
        "diagnostics":diagnostics
    }));
    assert_eq!(events.len(), 1, "an unexpanded parent must not become a live occurrence");
    assert_eq!(events[0].external_id, "q0163series_20261031T130000Z");
    assert_eq!(diagnostics.len(), 1, "the unsupported parent needs an explicit outcome");
}
```

SHA-256 pins: probe source `06e09faf3aa19aa64e90880d8721699a8de12407e658d21576ae5b780181b8f7`; raw fixture as compact sorted-key JSON without a final newline `513bae895409772a52f056574a6fd7742272a8984ba60566a45af5ad524cff85`; `src/services/calendar_wire.rs` `72bd39a301f22bc616687683cfb83dd0ebf8b8d30f601ece070f85d470e13cd3`; `Cargo.lock` `75aa5481baa9fddc7ef9b9e084fd76d8336b24de09e2797ec0d056adef5dd884`.

**Bounded implementation handoff.** Amend this existing unsupported-input ticket to reject live entries carrying recurrence rules in `calendar_wire::parse_event_with_diagnostic` before conversion to `DesiredEvent` loses that evidence. Produce an explicit unsupported-input diagnostic while preserving valid expanded instances. Add the retained regression to `tests/calendar_wire.rs`, then exercise the U3 wire-to-capture row below: no Task for the parent, the supported instance still captures, and the affected manifest scope remains uncovered with residual protection. Rejection supplies no authority to retire a previously admitted Task. Attach the implementation revision, exact command and results to the filed ticket; no ticket filing or repair is established here. This implements the existing U3 requirement under `UBU-D0318`.

Use a disposable synthetic calendar and durable store with the clock fixed at `2026-10-31T12:00:00Z` and planning horizon `[2026-10-31T12:00, 2026-11-02T12:00)` UTC. The source calendar zone is `America/New_York`, whose offset changes from `-04:00` to `-05:00` at `2026-11-01T06:00:00Z`; pin the time-library/tzdata version in the fixture digest. Seed opaque expanded instances with distinct source occurrence ids through the production wire/capture path, pinning raw `dateTime` strings and expected UTC occupancy. Per the events.list reference cited above, `timeMin` filters on event end and `timeMax` on event start, so a bounded read over the horizon delivers E1 and E2; record the parameters used. Isolate the slice from the workaround blocker.

| Instance | Raw-wire start and end | Expected UTC occupancy |
| --- | --- | --- |
| D1 | `2026-10-31T09:00:00-04:00`, `2026-10-31T10:00:00-04:00` | `[2026-10-31T13:00, 14:00)` |
| D2 | `2026-11-01T09:00:00-05:00`, `2026-11-01T10:00:00-05:00` | `[2026-11-01T14:00, 15:00)`; same wall-clock text as D1, different instant |
| T1 | `2026-11-01T01:30:00-04:00`, `2026-11-01T02:30:00-04:00` | `[2026-11-01T05:30, 06:30)` |
| T2 | `2026-11-01T01:30:00-05:00`, `2026-11-01T02:30:00-05:00` | `[2026-11-01T06:30, 07:30)`; identical wall-clock text to T1, distinct occurrence |
| E1 | `2026-10-31T07:30:00-04:00`, `2026-10-31T08:30:00-04:00` | `[2026-10-31T11:30, 12:30)`, straddling the horizon start |
| E2 | `2026-11-02T06:30:00-05:00`, `2026-11-02T07:30:00-05:00` | `[2026-11-02T11:30, 12:30)`, straddling the horizon end |
| U1, U2, U3 | An all-day `date` entry on `2026-11-01`; a `dateTime` without offset; an entry carrying `recurrence` rules | None: explicit unsupported-input outcomes with the affected scope recorded as uncovered |

| Check | Required result |
| --- | --- |
| Capture every instance, repeat capture, reopen the store and recapture | One Task lineage per occurrence. Each validated window equals the pinned interval: D2 at `[14:00, 15:00)` rather than `[13:00, 14:00)`, T1/T2 as two Tasks on disjoint intervals, E1/E2 unclipped. Wall-clock equality creates neither a merge nor a conflict. |
| Deliver the same entries with a `timeZone` field or calendar default zone that disagrees with the explicit offset | The explicit offset governs occupancy and the zone field shifts no interval. Re-resolving wall-clock text through the zone database shows as a mismatch against the pinned intervals. |
| Supply U1, U2 and U3 alongside D1 | D1 captures; each unsupported entry produces an explicit diagnostic and no Task, and U3 is never captured as a single occurrence at its first start. The manifest records each affected scope as uncovered, so certification requires residual protection there. Whether all-day entries should occupy capacity is not decided here. |
| Truncate the page chain, interrupt capture before and after a durable Task commit, or request `timeMin`/`timeMax` that exclude E1 or E2 | Delivered occurrences may remain captured, but the manifest cannot certify coverage; request parameters and the complete page chain are completeness evidence. An occurrence excluded by the request leaves its interval uncovered, not free. |
| Certify coverage, advance the clock to `2026-11-02T12:00:00Z` with horizon `[2026-11-02T12:00, 2026-11-04T12:00)`, and add D3 at `[2026-11-02T14:00, 15:00)` UTC to the source before any recapture | The prior manifest's horizon binding is stale: affected capacity is neither certified nor free until recapture and a renewed coverage check. After recapture, D3 occupies its interval and E2 straddles the new horizon start. A missed scheduled capture is the same stale state, not evidence of free time. |

On the CPU reference path and each enabled planner backend, 30-minute Dynamic probes must fit in none of `[13:15, 13:45)` on 2026-10-31, `[14:15, 14:45)`, `[05:45, 06:15)` and `[06:45, 07:15)` on 2026-11-01, `[12:00, 12:30)` on 2026-10-31 and `[11:30, 12:00)` on 2026-11-02. Controls with certified feasible placements: `[13:15, 13:45)` on 2026-11-01, which D2 would occupy under D1's stale offset; `[12:30, 13:00)` on 2026-10-31; `[07:30, 08:00)` on 2026-11-01. These probes verify intervals, not multiplicity; the mixed-blocker slice retains the capacity-input assertions.

Attach implementation revision, exact commands, raw-wire fixture digests with time-library/tzdata pins, request parameters and page chains, backend/configuration identities, diagnostic outcomes, Task/version lineage, manifest certification results and probe outcomes to the filed ticket and scoped manifest. Passing this slice covers only the exercised zone, transition and input kinds; the operator's actual source zones and the remaining `UBU-D0318` gates still require evidence and approval. No filing or passing result is recorded here.

### Resolution

Open. `UBU-D0318` remains the retirement specification. The source audits, proposed cancellation remediation ticket with its payload-minimization/replay extension, Google read-path qualification, bounded cancellation slice, separate series-truncation follow-up, mixed-blocker handoff ticket and timezone/horizon-crossing/unsupported-input slice identify implementation work and required evidence; none supplies a completed acceptance slice. Every `UBU-D0318` evidence bullet now maps to at least one bounded slice or existing assertion source above; add specification only where an implementation result exposes a gap. Filing and implementing the repair, qualified cancellation delivery/ordering and any cursor recovery, actual scoped source/series coverage, confirmation of deployed paths/revisions, the remaining required checks and the operator-approved blocker handoff remain outstanding. The executed U3 parser regression identifies a concrete repair within the existing unsupported-input ticket; its admission/coverage check remains outstanding. This documentation change removes no blocker.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `eb861e919fecf938de4defc50d2e208a61610dba` for provenance. It does not
  define the patch's file contents: do not use `git show HEAD:<path>` or the Git index
  as the original version when it differs from the working tree.
- Use exact working-tree lines for all removed and unchanged lines. Preserve existing
  metadata updates and exclude unrelated edits from the proposal.
- To generate the diff, compare unmodified working-tree copies with the proposed
  contents. Validate against the working tree with `git apply --check`; a successful
  `git apply --check --cached` checks the index and is insufficient.

## Canonical design context

The excerpts below are **not** the whole canonical files. They are the subset selected by
following references from the selected question: its dependencies, the decisions those
questions cite, and the sections those cite in turn.

Read them under these rules:

- **Absence is not non-existence.** Ids and sections you cannot see here still exist.
  Never conclude that a question is unasked, a decision unmade, or a section absent
  because it is not shown.
- **Do not renumber or reuse ids.** Allocate new entries from the next free ids below.
- **Patch context must match the real file.** Hunks are applied against the full files,
  not against these excerpts, and `git apply` locates them by surrounding context. Quote
  surrounding lines exactly as they appear here.
- **Scope your edits to what you can see.** Do not delete or rewrite text on the
  assumption that it duplicates something outside these excerpts.

Next free ids — use these for anything you add:

- next question id: `UBU-Q0191`
- next decision id: `UBU-D0388`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DECISIONS.md — cited decisions

#### UBU-D0258

```markdown
## UBU-D0258: Phase 1b canonical mutations carry sync-ready envelopes

**Status:** Accepted → DESIGN.md §23.1. Resolves `UBU-Q0130`.

Every Phase 1b canonical mutation carries a mutation envelope before it enters the StateStore/admission writer. The required Phase 1b fields are `idempotency_key`, `observed_versions`, `origin_device_id`, `actor_identity_id`, `authority_source`, `created_time`, `effective_time`, and `recorded_time`; `observed_policy_versions` is also required whenever the mutation relies on Compartment, Zone, projection, routing, or other policy state. Optional execution-context and backend provenance may refine auditability but cannot replace origin Device, actor Identity, or authority source.

`observed_versions` is a map from canonical object id to the version reference observed for that object. The Phase 1b representation derives that reference from the existing per-object integer counter, serialized canonically as `v<N>` for existing objects and as an explicit absence precondition for creates or operations whose correctness depends on non-existence. It is a precondition set, not the ordering clock and not a global version.

Phase 1b mutation call sites do not name hybrid logical clocks, Lamport clocks, vector clocks, append-only logs, or content-addressed bundles. They ask an admission-owned causality/idempotency issuer for the envelope; that seam returns the frozen provenance, precondition, idempotency, timestamp, and opaque local-causality fields needed by the current single-Device reducer. Phase 2 may implement the same seam with the `UBU-D0247` hybrid stack, including HLC ticks and causal parents, without changing domain mutation APIs or reinterpreting existing Phase 1b fields.

`created_time`, `effective_time`, and `recorded_time` are distinct. `created_time` records when the mutation artifact was first assembled, `effective_time` records the domain time the mutation asserts for the changed fact or decision, and `recorded_time` records when the controlling Device durably records the mutation for admission. If an unattended advisory batch proposes a candidate at 02:00 and the operator admits it at 09:00, the admitted mutation may preserve 02:00 as created/effective time while using 09:00 as recorded time.

Within one origin Device, `(origin_device_id, idempotency_key)` is the duplicate-detection key. Replaying it with the same canonical payload returns the previously recorded result and performs no new write, version increment, side-effect emission, projection push, or derived-state invalidation. Replaying it with a different canonical payload is an `idempotency_key_conflict`; the new payload is not admitted except through an explicit conflict-resolution mutation.

Derived artifacts such as Plans, Calendars, reports, risk summaries, and projection previews are `derived_state`. They carry lineage, provenance, input digests, freshness, and invalidation metadata, but not the canonical mutation envelope unless UbU admits the artifact itself as canonical state. The mutation that records, invalidates, or publishes such an artifact carries the envelope.

---
```

#### UBU-D0259

```markdown
## UBU-D0259: Phase 1b tombstones retain structural identity and admit best-effort purge

**Status:** Accepted → DESIGN.md §23.1. Resolves `UBU-Q0132`.

Phase 1b canonical deletion, retirement, removal, and admitted candidate discard are represented by a tombstone mutation and retained tombstone record, not by hard deletion of the canonical object row. A tombstone is canonical lifecycle state: it says the object id is no longer active or admissible while preserving enough structure for causality, idempotency, undo eligibility, conflict detection, audit, and projection cleanup.

The Phase 1b tombstone record retains only policy-safe structure: `object_id`, `object_kind`, the latest object version reference, tombstone lifecycle state and reason code, created/effective/recorded lifecycle timestamps, origin/provenance or mutation-envelope reference, observed-version and observed-policy-version refs needed to validate the tombstone, policy-safe Compartment refs required for local enforcement, optional superseding/decomposition refs, and projection-deletion obligation refs. It does not retain active payload fields such as titles, notes, private evidence, source snippets, human-readable Compartment labels, sensitive reason strings, cached explanations, or derived summaries when those fields are subject to purge or redaction.

Every tombstone is admitted through the `UBU-D0258` mutation-envelope path. The envelope belongs to the tombstone mutation; the durable tombstone stores or references the envelope fields needed for replay and audit rather than existing as an unenveloped side table update. Replaying the same tombstone idempotency key with the same canonical tombstone payload returns the existing result and must not emit duplicate projection deletion attempts.

**Decomposition-undo rule deprecated by `UBU-D0278`.** The paragraph below is retained as the superseded rule, not an instruction to clear a parent's tombstone. Undo is a new admitted structural replacement: it creates a restored Task with a new handle, marks the Container and children superseded or moot, and retains the original Task's retirement and history. The remaining tombstone and purge policy in this record stands.

> Decomposition parent retirement becomes a tombstone of the parent Task plus the existing decomposition record's parent snapshot. Undo is an admitted mutation that observes the tombstone and the decomposition record, clears the tombstone only if policy still permits restoration and an authorized restorable snapshot payload is still present, and increments the parent object's version. If the parent payload or snapshot has been physically purged or redacted beyond restoration, undo must not synthesize the old payload from the tombstone; it may instead create a repair or clarification path requiring new user input.

A best-effort physical purge may remove payload blobs, snapshots, derived caches, local projection payload, and external-handle material after or alongside tombstoning, but it does not erase the minimal tombstone while that record is needed for causality, idempotency, audit, or retry. If policy later requires the tombstone itself to be redacted for a Device, the Device receives only an allowed structural representation or an opaque redacted handle; it must not receive forbidden Compartment ids or labels.

External projections are separate `projection_state`. Tombstoning a canonical object may enqueue idempotent projection-deletion obligations keyed by projection surface, external object reference, canonical object id, and tombstone mutation, but success or failure of those queued deletions does not determine whether the canonical object is tombstoned. Projection deletion retries may keep only the minimum external handle allowed by policy, and projection diagnostics must remain structural if payload or Compartment identity is restricted.

---
```

#### UBU-D0318

```markdown
## UBU-D0318: Recurring-busy blockers retire only after scoped capture and occupancy verification

**Status:** Accepted → DESIGN.md §1. Specifies the retirement process for `UBU-Q0163`; implementation evidence and the operator's scoped retirement remain open.

Adopt the coverage manifest, stable occurrence lineage, capacity checks and reviewed blocker handoff in DESIGN.md §1. Existing timed expanded-instance capture is the baseline, not evidence that all recurrence is unsupported or that every relevant commitment is covered. Series-level import is optional when expanded instances demonstrably cover the reviewed scope. Keep residual blockers for unsupported or unverified intervals.

Before any scoped retirement, attach revision-pinned behavioral evidence demonstrating:

- Expanded-instance capture and repeated/on-demand capture preserve one Task lineage per occurrence across retry and restart. Different occurrences remain distinct; single-occurrence and series edits, cancellations and explicit exceptions used by the reviewed scope remove stale future occupancy without rewriting logged history.
- Timezone/DST resolution and horizon-crossing intervals match expected UTC occupancy. All-day, floating-time, unexpanded series and other unsupported inputs produce explicit outcomes with retained protection; interrupted or incomplete capture cannot certify coverage. Horizon advancement repeats the coverage check.
- The CPU reference path and enabled planner backends agree on busy/free capacity and reject flexible placement inside fixed commitments. Replacement Tasks, their projections and workaround blockers are not counted twice; genuine commitment overlaps remain conflicts.
- A mixed covered/uncovered blocker retains exactly the required residual protection. Approved handoff, interrupted handoff, stale-version refusal, restart and replay preserve reservations, invalidate old Plans and prevent reuse/projection until recalculation is certified.

These are required implementation and retirement checks, not passing results. Closing `UBU-Q0163` requires the source/path inventory, these results and the recorded operator-approved handoff for the identified workaround.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §1 — Overview

```markdown
## 1. Overview

**UbU** is a privacy-first planning, coordination, and self-governance system for implementing life logistics.

UbU takes messy real-world inputs—tasks, calendar events, messages, external events, user preferences, physical/emotional state, Resource availability, Skill capability, integration data, realtime interaction streams, and agent or worker outputs—and turns them into explicit, inspectable, recalculable Plans.

UbU is not merely a task list or calendar application. Its core motivation is to help an individual human being plan and implement the logistics of an actual life: what matters, what must happen, what the world must contain, what the user is capable of doing, what can be learned, what should be bought or delegated, and what should happen next. Organizational coordination is an important emergent property of the same model, not the root purpose.

The canonical UbU use-case formula is:

> **Objective + Current State + Constraints + Resources + Skills + Techniques + Preferences + External Options → Legitimate Plan**

The canonical use-case statement: UbU helps an individual user solve everyday life problems by transforming a desired outcome into a legitimate, executable plan. It does this by modeling the current state of the user's life, the desired state, required Tasks, available and missing Resources, available and missing Skills, reusable Techniques, affect and energy constraints, financial tradeoffs, public or marketplace options, expert-guided alternatives, and evidence from execution. UbU's purpose is not merely to schedule time, but to help the user determine what path can realistically move life from the current state toward the desired state.

The deeper purpose is to help people build the capabilities, resources, routines, and relationships needed to actually live the life they choose — not just to schedule it.

**Access within ordinary life logistics (`UBU-D0338`).** Present guided camera acquisition and voice/text/form choices as ordinary ways to capture concerns, review proposals and act on Plans. Keep UbU's primary identity as a privacy-first life-logistics system; access features are available without a disability label or special onboarding category. Describe concrete benefits and limitations for tested workflows and Device/assistive-technology combinations. Camera availability alone establishes no accessibility claim.

The first MVP is designed around **dogfooding**: using UbU to coordinate the design, development, release, and maintenance of UbU itself.

Recent LLM and agentic-AI changes reinforce UbU's core boundary: realtime, multimodal, tool-using, memory-bearing models are valuable interaction and extraction backends, but UbU remains the user-sovereign state-transition, planning, logging, privacy, and review layer.

**Advisory generation policy (`UBU-D0341`).** For the MVP/Phase 2 advisory boundary, the operator configures sampling behavior in the selected inference runtime. UbU adds no generic decode Settings or application-selected temperature, top-p/top-k or repetition penalties, hidden request overrides or automatic retry tuning. Existing model/endpoint selection, timeout and review seed/ceiling behavior remain; the review seed provides no cross-model or cross-runtime reproducibility guarantee.

UbU owns bounded validator-safe producer grammars (`UBU-D0302`), finite request/output and job budgets, cancellation, validation and ordinary candidate review/admission. Required grammar and resource/output-limit fields may be sent to the runtime without transferring these contracts to sampler policy. A runtime unable to meet them leaves the affected operation unavailable with an explicit limitation and eligible retry/manual route. Abort, timeout and invalid output preserve saved state and admit no generated state changes. Retries retain current authority/input bindings and finite call/time budgets; failure cannot relax validation or authorize another route.

Document each supported adapter's required grammar/limit behavior and runtime sampling configuration instructions. Pin observable sampling configuration or its digest in qualified Device/backend profiles and label unknown effective defaults explicitly. Configuration changes require affected qualification checks under `UBU-D0331`/`UBU-D0336` where applicable. Before claiming conformance, require adapter fixtures that inspect requests for absence of application-selected sampler overrides while preserving existing review seed/ceiling behavior and required grammar/limit fields; exercise abort, timeout and invalid-output paths for preserved saved state, bounded retries and unchanged review/admission requirements. These are required checks, not passing runtime evidence or a diagnosis of the Vocabulary abort.

For Phase 1, UbU must also be understandable as a first-person user experience. The minimal user-facing loop is: answer a small number of bootstrapping questions, allow UbU to construct an initial context model, receive one recommended next Task, inspect why that Task matters now, act or override, then let UbU learn from the result through Logs, Snapshots, and recalculation.

For Phase 2, a single-live-session sync implementation is sufficient (`UBU-D0312`), with arbitrary N Devices, concurrent offline mutations, partial replicas, and transport-independent integrity-protected statements. Direct exchanges, imported bundles, and user-selected encrypted stores share the same causality, policy, and conflict rules; no Device or transport becomes a permanent canonical authority. Full automatic peer mesh is optional. DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26's topology research remains a freeze gate, with at least three Devices demonstrating serialized direct exchange and encrypted store-and-forward, recoverable delivery, deterministic admission/conflict handling, and Compartment-safe partial replication as specified by `UBU-D0312`.

The minimum indirect path for the first Phase 2 sync release is implemented, authenticated encrypted bundle export/import through a user-selected file store, with at least one documented external rclone or rsync copy workflow exercised before freeze (`UBU-D0313`). Manual export/copy/import is sufficient; built-in clients and background delivery are optional. Encryption and recipient authorization protect statements, payloads, and manifests before storage, while ordinary admission and Compartment policy govern every import.

**Interrupted sync recovery (`UBU-D0314`).** Maintain a durable, policy-protected recovery record throughout each exchange. Connection loss, interrupted import, or restart without an admitted final checkpoint for the declared exchange scope records `incomplete_sync_session`. End the failed live session and release its slot while preserving recovery records and queued work; other Devices may synchronize and ordinary policy-authorized offline work may continue against an explicitly stale local view.

Fully verified, authorized statements whose required causal/admission dependencies are satisfied may be admitted incrementally. Persist each admission outcome, resulting state, and deduplication metadata as one crash-consistent unit. These admissions survive disconnect, retry, and discard, subject to existing redaction/purge policy. Partial statements, unverified payloads, and items with missing required statements or dependencies remain staged; they cannot be treated as admitted mutations or supply authoritative inputs to Plans or external projections.

An integrity-protected `checkpoint` or equivalent authenticated manifest commit must bind the declared exchange scope and expected statement/payload inventory, or its content-addressed equivalent. Admit it only after all material required for this Replica's authorized replication level has been durably verified and the required dependency/admission checks have completed. Report completion only after that checkpoint admission is durable, and only for that local scope. A checkpoint cannot admit contested mutations or prove other Devices current. A lost acknowledgment leaves delivery unknown to the sender; replay cannot undo the recipient's durable admissions. Outgoing statements remain pending for an intended recipient until the sender durably records a matching authenticated acknowledgment of that recipient's durable checkpoint admission. Exchange acknowledgments directly or through encrypted return bundles; transmission, file-copy success, or local checkpoint creation is insufficient.

The recovery record tracks exchange/bundle identity, source/recipient Device references, authenticated manifest reference if available, last locally admitted checkpoint, received/missing content references, and durable per-statement admission/idempotency outcomes. Reconstruct progress after restart from durable records. Recovery storage, dependency fetches, retention, and diagnostics obey current Zone/Compartment policy; restricted views reveal neither denied payload nor Compartment identity. Retention cleanup preserves permitted causality, deduplication, and incomplete/discard outcome metadata; missing content must be retransmitted rather than assumed received.

At the next authorized connection or import, resume from durable progress, or replay the original statements if no usable cursor exists. Preserve original statement ids, content hashes, idempotency keys, causal parents, and observed versions. Recheck integrity, Device trust, actor authority, current routing/Compartment policy, object/policy versions, and dependencies before admitting pending material or disclosing retained content. Recovery records and old checkpoints grant no authority. Automatic recovery is allowed only for deterministic, idempotent, non-authority-changing cases under `UBU-D0248`; revoked Devices and review-required conflicts retain their ordinary rejection/review rules.

Offer Resume/retry and Discard through the existing policy-safe incomplete/stale-sync diagnostic, showing the last completed scope and permitted pending counts. Discard removes unfinished incoming staging and its cursor while preserving already-admitted history, locally authored queued work, and still-required policy-permitted causality/idempotency metadata; record the discard outcome. It does not emit a tombstone or undo admitted mutations. A transient interruption alone requires no blocking merge prompt. Review-required conflicts use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §17's blocking `manual_decision` diagnostic and admitted `conflict_resolution` statement. Apply DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §22's Plan invalidation/recalculation rules to admitted changes even while the exchange remains incomplete.

**Sync-state warnings (`UBU-D0315`).** Multiple Devices may remain powered on and perform policy-authorized offline work simultaneously; only live sync exchanges serialize under `UBU-D0312`. Explain this once in Phase 2 onboarding. Ordinary offline use, Device switching, peer absence, and elapsed time alone cause no modal warning or recurring notification. Time-based reminders are opt-in with a user-selected cadence.

Keep one compact, non-blocking status for the active permitted sync scope, with details on demand: last durably admitted checkpoint and its scope/time (or "No completed exchange yet"), permitted locally authored pending counts split into awaiting exchange and awaiting recipient acknowledgment, staged incoming work, and incomplete-session state with Resume/retry and Discard per `UBU-D0314`. Say "Delivery unconfirmed" for a missing acknowledgment. File-copy success and a completed local exchange never justify "All Devices are up to date". A disconnected peer's changes are unknown unless policy-permitted evidence establishes pending state; age alone proves neither new changes nor their absence.

Show an inline Plan/Calendar freshness explanation where relevant inputs may be missing or stale: "This Plan uses this Device's current view; other Devices may have changes." Identify confirmed remote pending changes only when supported by permitted evidence. Locally authored outgoing work is saved local state, not evidence that the local Plan is stale. Label protected blocks and policy-withheld items separately from exchange failures; retrying sync cannot expand visibility.

Before external projection whose relevant permitted inputs have known pending changes, an incomplete exchange, or unknown peer freshness, present an inline review: "Other Device changes may be missing. Synchronize before projection, or use this Device's current view." Offer Sync/exchange (including export/import), Continue with current view, and Cancel. Continue requires explicit stale-state acceptance once per projection operation or reviewed batch, bound to the Device, operation, payload/input and policy versions, and permitted scope. Record it under §17.0 using `decision_recorded` with `decision_kind: safeguard_advisory_bypassed`; recheck the bindings before execution. This accepts freshness uncertainty only. Current authority, Device trust, integrity, Compartment/export policy, Plan-legitimacy requirements, and `UBU-D0248`'s blocking `manual_decision`/`conflict_resolution` rules still govern eligibility. If those rules forbid execution, omit Continue. Mere interruption or missing peer contact creates no blocking merge prompt.

Coalesce advisory explanations by Device, permitted scope, condition, and material evidence. Offer Dismiss until material change or Snooze until a user-selected time; persist the local choice across views and restart while keeping status discoverable. Reopen for material changes affecting that warning, such as newly learned permitted pending statements or changed relevant Plan/input versions; snooze may also reopen at its chosen deadline. A new projection operation needs its own scoped choice. Age or restart alone cannot reopen a dismissed advisory; opt-in reminders may follow their chosen cadence. Dismissal needs no justification and is logged under §17.0 without turning the warning into a synchronized Task. Advisory suppression cannot hide a required blocking conflict diagnostic.

Generate visible text, counts, urgency, and reappearance only from evidence or derived warnings allowed at this Replica's replication level. Restricted notices use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §23's "A protected object cannot sync to this Device." without denied payload, source object identifiers, or Compartment ids/labels; omit object-specific notices when even existence is denied. Warnings preserve intentional partial replication and never recommend powering off other Devices or switching to a single primary Device.

**Future schedulable manual review (`UBU-D0316`).** A later versioned extension may admit a shared Dynamic review Task with independent, coherent scheduling inputs and a protected conflict-episode identity. Its target is evidence to review, not admitted contested effects. Place it only in validated unaffected Calendar capacity; otherwise keep it unplaced. The diagnostic/prompt must still admit a permitted source-conflict `conflict_resolution` without review Task creation, placement, or completion. Conflicts in the Task, its scheduling/projection artifacts, or resolution workflow terminate at that diagnostic and never spawn review-of-review Tasks. Task lifecycle reconciliation follows resolution; snooze or deletion cannot clear the underlying blocker. Current authority and source privacy govern every step. The first Phase 2 sync release retains §17's diagnostic-only model until this extension's versioned contracts and behavioral gates pass.

**Task lore after Clarify (`UBU-D0317`).** After a Clarify round, persist the operator's submitted answers through ordinary admission independently of synthesis, then offer one additional local synthesis job. The Task description is readable narrative. Keep the pre-existing description, subsequent operator edits, exact questions/answers and their necessary context as separate durable per-Task source records with stable ids, versions, actor/Device/session/round provenance, effective/recorded times, hashes and privacy/retention policy. Versioned context refs are sufficient only while the referenced context remains retrievable. Corrections append records with explicit supersession links; they never edit originals. Accepted model prose is a derived revision, not independent evidence. Raw audio/video retention is not required.

The local model returns an `AdvisoryCandidate` containing proposed prose, model/template provenance, the complete source manifest/digest and a source-to-prose coverage map. Every answer, qualification, original lore statement, correction and unresolved issue must map to prose spans; every factual prose assertion must map back to sources. Equivalent repetitions may share wording. Superseded assertions retain a temporal account of the old and corrected meanings; disagreements, unknowns, declined answers and open questions remain explicit. Preserve attribution, negation, conditions, quantities and uncertainty. Unsupported inferences remain separate labelled suggestions, not synthesized lore.

Review shows the description diff and a source-by-source checklist with exact source text beside its mapped prose. Deterministic checks require complete source/span coverage, valid references, finite bounds and current bindings; structural coverage and another model's opinion cannot establish semantic fidelity. Explicit operator approval confirms readable prose preserves all source meanings and invents no facts or agreement. Offer edit, regenerate or keep the prior description; edited prose needs renewed coverage review. Recheck authority, policy, Task-description/source/candidate versions and source digest before atomically admitting the description revision and its provenance. Concurrent changes invalidate approval and use ordinary conflict review. Failure, rejection or model unavailability preserves the prior description and saved answers. Lore approval does not admit typed planner constraints or authorize external action.

Later Clarify uses admitted prose plus the source manifest, unresolved issues and correction links. Supply exact question/context/answer records for topics being revisited and make remaining permitted originals retrievable. Check the originals before treating a topic as settled; absence from a prompt is not absence of knowledge. Missing records make context explicitly incomplete. A partial Replica unable to establish the full source frontier may offer a scoped preview but cannot replace the Task description.

Declare finite storage, prompt, result and job time/call limits. Retain source history and admitted revision/provenance while the Task is retained; archival must remain queryable. Chunk long inputs along complete source records and track coverage across chunks through the final prose. If complete reviewable prose cannot fit, stop and keep saved sources rather than truncate or drop knowledge; no cloud fallback. At storage exhaustion, pause Clarify and show any unsaved input; never claim it saved or silently evict history. Rejected candidate payloads follow the ordinary minimized retention lifecycle.

Source records, synthesized prose, coverage maps and subsequent context inherit all source disclosure, routing and retention restrictions; local processing does not waive them. Reject synthesis into a Task whose destination policy cannot protect its sources. Recheck policy on every read, sync and export. Authorized redaction/purge also removes disallowed content from dependent prose, revisions and cached context; prevent reconstruction from retained derivatives. Keep only permitted minimized gap metadata, disclose incompleteness without revealing denied existence, and never claim lossless retention beyond that scope.

**Recurring-busy workaround retirement (`UBU-D0318`).** Timed expanded recurrence instances already capture as individual Tasks. Retire a temporary manual capacity blocker only for an operator-reviewed source/series and planning scope with verified replacement occupancy. Full series-level import is not a prerequisite when expanded instances cover that scope; unsupported inputs, missed/on-demand capture and incomplete coverage retain explicit residual blocking.

Record a policy-permitted coverage manifest binding the implementation revision, source/series references, capture window/cursor and completeness evidence, source and admitted input versions, planning horizon, expected busy UTC intervals, occurrence-to-Task lineage, and blocker references. Every relevant busy occurrence must map to one active capacity-bearing representation; record how any uncovered part of the blocker remains protected. Titles or coincident times do not establish occurrence identity. Repeated capture, restart and replay preserve lineage without duplicate Tasks. Moves, cancellations and series changes supersede prior active reservations through ordinary admission while preserving logged history; absence from an incomplete response cannot establish cancellation.

Fixed busy commitments use capacity-occupying Static Tasks at validated UTC start/end times and bound chunks under `UBU-D0279`. Capture counts, Calendar display and Task existence alone do not prove occupancy. Verify the CPU reference path and each enabled planner backend against expected occupied/free intervals: flexible placements cannot enter busy time, and a source occurrence, its projection and its temporary blocker cannot consume capacity twice. Distinct commitments that genuinely overlap retain their conflict rather than being deduplicated by time.

Before removal, show the exact blocker replacements and residual intervals and obtain explicit operator approval bound to the manifest and current versions. Apply them through ordinary admission as one batch, or an equivalent serialized handoff that prevents Plan certification/projection of intermediate missing or doubled occupancy. Invalidate affected Plans and certify recalculation before reuse or projection. Failure or stale inputs preserve protection and require fresh review. Retire only the identified workaround, preserving source commitments and history. Advancing the horizon or receiving material source changes requires renewed coverage checks before using affected capacity; missing, stale or policy-withheld data never proves free time. Evidence and diagnostics obey current Compartment policy; this process neither grants capture/export authority nor bypasses existing sync/projection checks.

**Qualification phase (`UBU-D0384`).** The hardware, runtime, route and cost qualification below (`UBU-D0331`–`UBU-D0382` for `UBU-Q0177` and `UBU-Q0178`) is Phase 3 work: it closes on real-device evidence, not on further specification. Phase 2 builds the capture slices of §4.2 without enabling them.

**Measured local multimodal hardware (`UBU-D0331`).** Qualify an exact physical-hardware/software profile, separately identifying every execution-enclave Device and its capture, review, inference and optional planning roles. Pin hardware/RAM, OS build and security-update posture, drivers/runtime, model and weights digest, quantization, context/media sizes, storage/key configuration, UbU/template and policy revisions. Declare enabled sources, latency classes, concurrency, supported session duration and operating conditions. Primary mobile interaction does not select a full mobile planner or compute API (`UBU-Q0160`); capture-mode/device allocation and delivery sequence remain `UBU-Q0183`.

Before testing, freeze the scenario set, useful-outcome rubric and finite acceptance limits for quality, latency/freshness, memory/storage, battery/thermal behavior and cancellation. Use §21.1.3's ordinary interaction and text/form workflows, with synthetic or expressly consented fixtures covering visible details, exact names/values, noisy speech, ambiguity, corrections and unsupported requests. Review grounding, preserved uncertainty, fabricated assertions, correction burden and useful candidate-to-admission/next-action outcomes; transcripts, token rates and plausible conversation alone cannot pass. Count accepted, corrected, unresolved, rejected and failed outcomes separately over all trials. Numeric interaction profiles follow §21.1.4 and remain `UBU-Q0168`'s measured work.

For each proposed Device/backend/mode, run at least 30 cold-start and 100 warm trials using the pinned fixtures, plus at least 30 minutes of its proposed concurrent capture/inference/UI workload and the full maximum supported session if longer. Measure the complete capture-to-review route, including any authorized worker transfer. Record ambient conditions, charging/battery state, competing load, instrumentation and repeat procedure. Report §21.1.4's separate timings with sample counts, median, p95, maximum and misses/errors; include peak aggregate CPU/accelerator memory, model loading, buffers/caches and concurrent manual/planning work at the declared state/context limits. Report model/state/approved-artifact storage, peak scratch and required free-space reserve, energy/battery measurements and uncertainty, temperature/throttling and performance over time. Unavailable metrics stay explicit; missing evidence for a required limit prevents qualification. A short successful run cannot establish sustained or longer-session support.

Endpoint qualification must demonstrate a maintained security-update posture, installation/model integrity checks, OS access isolation, protected Device keys and access-controlled encrypted persistent storage with documented key custody and recovery. State the threat envelope: locked-device loss, unauthorized unprivileged processes, unintended persistence and unauthorized egress; do not claim protection from a compromised OS or malicious local administrator. Exercise source/Device revocation, lock/restart, denied reads/egress, storage exhaustion and interrupted cleanup. Inventory raw/intermediate/retained copies, OS/runtime swap, hibernation, crash dumps, caches and backups; enforce §12.2 and §21.1.5 on every applicable path or disable the affected mode. Encryption alone cannot satisfy transient-media retention. A rooted/debug planning-parity device supplies no endpoint-security evidence by itself; hardware attestation remains optional under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26, and cleanup establishes no forensic-erasure guarantee.

Publish a policy-safe support matrix linking the exact profile, enabled workflows/classes, measured usable-memory/free-storage envelope, quality and timing results, sustained operating duration, security/retention outcomes and demonstrated fallback. Test offline/model loss, low battery, thermal/resource pressure, stop/silence, delayed results and manual completion without hidden cloud routing or loss of permitted saved state. A worker on another Device needs its own qualified endpoint/profile and existing route/Compartment checks; a same-Device worker remains an execution context unless separately admitted under §23.1. Ownership grants no disclosure authority. Only profiles passing all declared limits and mandatory security/retention gates may be operator-approved for those slices; material profile changes require affected rechecks. Label the selected configuration a tested sufficient profile among the tested candidates, with failures and limitations visible; infer no universal phone minimum or local-versus-cloud quality ranking. `UBU-Q0177` stays Open until a reviewed evidence-backed profile is selected; a protocol alone cannot close it. Lower-cost reach remains `UBU-Q0178`.

**Inference runtime reuse qualification (`UBU-D0336`).** Extend the endpoint gate to the deployed runtime's effective process permissions and source-dependent state across jobs. Inventory prompt/KV caches, embeddings, decoded media, buffers, leased handles and scratch separately from verified read-only model weights. A warm process or shared weights grant no access to prior job context. Enforce §21.1.5's lifecycle before reuse; separately authorized retained artifacts enter a new job only through fresh broker access and current bindings. If safe reuse cannot be demonstrated, qualify an isolated per-job runtime or leave the affected mode unavailable.

Run paired synthetic camera-and-voice jobs A/B under distinct grants, with B denied A's source and context. Put a unique synthetic detail in A and request it from B without supplying it. Exercise both job orders, ordinary completion, cancellation and source/policy revocation, crash/restart and resource pressure; include overlapping jobs only where concurrency is proposed. Inspect B's actual supplied context, runtime reads, result/artifact releases, cache/handle lookups and diagnostics for unauthorized A content or identifiers. Probe ambient filesystem, credential, camera/microphone and network access through the runtime's actual interfaces under deployment permissions; model refusal or a configured permission declaration alone cannot establish denial. Include an authorized control supplying the synthetic detail to B through the broker to verify the detector and permitted access path. Absence of a detail in generated prose alone cannot prove isolation.

Record pinned fixture/grant versions, expected and observed access/release outcomes, runtime permission observations, reset/termination behavior and policy-safe evidence links in the qualification report. Every enabled reuse/concurrency mode must demonstrate the allowed control and zero unauthorized reads/releases in these fixtures; unobserved mandatory paths remain unqualified. Repeat the cold/warm and sustained workload measurements with the enforced isolation, reset and cache restrictions active, accounting for their time, energy and aggregate memory/storage costs. Publish limitations beside the support matrix. These fixtures supply no passing runtime, hardware selection or forensic-erasure result.

**MVP hardware support coverage (`UBU-D0362`).** Select a set of qualified hardware/software profiles that collectively covers every mandatory capture/review slice in §4.2 (`UBU-D0339`): bounded laptop/desktop camera-and-voice, bounded phone capture/review with its selected inference route, and finite laptop/desktop realtime audio/video. For each required slice, the support matrix identifies the exact capture, review and inference Device profiles, any inter-Device route, enabled sources/classes, declared concurrency and session limits, passing whole-route evidence and operator approval bound to those revisions. Shared physical hardware may cover several slices; qualify the proposed combined workload or enforce and demonstrate the declared mode exclusions under `UBU-D0331`/`UBU-D0336`.

Record pass, fail or missing evidence for each mandatory slice and each required endpoint/route gate. Operator-approved passing configurations may establish support for their qualified slices while incomplete coverage remains visible. Close `UBU-Q0177` only after the operator approves a complete support set with passing evidence for every mandatory slice; a bounded camera-and-voice result alone cannot establish the realtime slice. This clarifies hardware-selection completeness under the existing release scope and supplies no measured result or mobile planner/API selection.

**Complete local processing route (`UBU-D0368`).** For each proposed §4.2 slice, extend the qualification profile with a stage-by-stage manifest of all enabled capture/preprocessing, OCR/vision, speech recognition, embeddings, inference and optional speech-output components, including delegated OS services. Bind each applicable stage to its implementation/model pins, execution Device/profile, effective permissions, input/intermediate copies, persistence paths and allowed destinations. Source-processing traffic stays on qualified endpoints and any selected authenticated encrypted, policy-authorized worker route. Keep package/model acquisition and separately authorized sync/export distinct; none authorizes cloud inference, hidden fallback or source-bearing telemetry.

On the exact deployment with its egress restrictions active, exercise the frozen useful-outcome fixtures with Internet access available and blocked; preserve the selected permitted worker link in the latter condition. Also exercise unavailable local components, queued retries and restored connectivity. Inspect actual provider/service calls and network attempts/destinations for source content and intermediates, including services outside the main model process. Include a controlled denied-egress probe and, when selected, an authorized worker-transfer control to verify observation and enforcement. A successful networking-disabled run, a local API label or absence of a plaintext canary in encrypted traffic cannot establish connected-mode locality. Undeclared remote processing, unauthorized transmission or an unobserved required path withholds the affected mode; backend loss preserves permitted saved input and the existing manual route without remote substitution.

Attach policy-safe manifest and observation evidence with pass/fail/missing stage dispositions to the support matrix. Measure the complete pipeline with its deployed restrictions active under the existing cold/warm and sustained trials, including every enabled stage's aggregate resource and retention costs. Apply affected rechecks when a stage, delegated provider, destination or enforcement configuration changes. These requirements supply no passing deployment or supported hardware; `UBU-Q0177` stays Open pending the existing complete-support-set evidence and operator approval.

**Qualification bindings at use (`UBU-D0375`).** Bind enabled multimodal modes to the operator-approved profile and complete pipeline manifest. Before source acquisition and inference dispatch, compare the effective deployment with its required pins under `UBU-D0331`/`UBU-D0336`/`UBU-D0368` across every execution endpoint, including actual model identity, delegated providers, effective permissions and retention/egress controls. Record how each required binding is verified. Missing or mismatched required bindings withhold every dependent mode; a stable pathname, backend label or past passing result cannot establish a match.

During each job, pin verified artifacts and enforcement settings or detect changes and invalidate affected work before further source access or result release. Recheck the job's qualification bindings at release. On missing or mismatched bindings, apply existing interruption, cancellation and retention rules: stop affected acquisition/output, reject late live results, preserve permitted saved state and keep immediate controls and the manual loop available. A shared component change affects all dependent routes. Changed bindings require affected rechecks and renewed operator approval before use; restored support never resumes capture or replays work automatically.

Require actual-controller fixtures for an approved matching route, model replacement at the same path, changed delegated-provider/permission/egress settings, worker-profile mismatch, restart with stale approval and change between dispatch and result release. Verify observed source access, output suppression and dependent-mode unavailability, including useful manual completion. Repeat the existing cold/warm and sustained measurements with these controls active and publish their costs, failures and evidence in the support matrix. These requirements establish no supported deployment; `UBU-Q0177` remains Open.

**Observed multimodal resource release (`UBU-D0381`).** Extend `UBU-D0331`/`UBU-D0336` qualification to the actual lifecycle of source-dependent buffers, pending backend submissions and runtime/worker handles after timeout, stop, expiry or revocation. Local invalidation still blocks further reads and result release immediately. A cancelled status, cancellation acknowledgment, process exit or lost worker link alone cannot establish complete-route cleanup or resource release. Bind outstanding work and buffers to the original job generation; prevent their reuse until backend quiescence and required cleanup are demonstrated. Verified read-only weights retain `UBU-D0336`'s separate treatment.

Keep unresolved resources charged to whole-route memory/storage/concurrency limits, including any separately qualified fallback. Withhold further dispatch through the affected runtime instance while release is unconfirmed; immediate controls and useful manual completion must remain schedulable. This containment cannot extend source/intermediate retention or the existing finite cancellation-to-resource-release ceiling. A failed or unobserved required release/retention bound prevents qualification. For a disconnected worker, exercise endpoint-local enforcement and report remote release as unconfirmed until permitted endpoint evidence is available.

Extend the existing distinct-grant fixtures with delayed backend completion, repeated cancellation, teardown failure and late callbacks/writes while a subsequent job is requested. Freeze expected dispositions and observe actual dispatch, buffer ownership and denied reuse, not just generated output. Record invalidation, backend quiescence, cleanup and resource-release offsets separately, with pending-resource high-water marks. Controller injections alone supply no deployed backend-release evidence; demonstrate the supported lifecycle on the exact physical endpoints. Include unresolved work, teardown and any fallback in the existing cold/warm and sustained resource measurements and support matrix. These are runtime-lifecycle observations, not forensic-erasure guarantees or passing hardware evidence; `UBU-Q0177` remains Open.

**Economically accessible local paths (`UBU-D0332`).** Compare two candidate routes for a bounded camera-and-voice slice under `UBU-D0331`:

- **Existing single Device:** bounded still-image capture and push-to-talk audio, with smaller/quantized or task-specific local models capable of CPU inference, finite context and serialized inference jobs. Evaluate supported older hardware without assuming that age, nominal RAM or model size establishes eligibility.
- **Existing personal worker:** capture and review on one Device, with bounded inference on an existing operator-owned laptop/desktop worker. Qualify both endpoints and the complete authenticated encrypted transfer route. Current Device trust, Compartment/source permissions and original consent, freshness, retention and job limits govern every copy; ownership or physical proximity grants no authority. Worker loss leaves a qualified same-Device route or useful text/forms.

Each candidate uses legally locally runnable software/weights with disclosed licenses, installation downloads and fees, and requires no hosted inference, paid API credential or recurring model-service subscription. Dedicated accelerators and continuous streaming are optional for this evaluated slice; `UBU-Q0183` retains overall release scope and sequencing. Demonstrate same-Device operation with networking disabled and worker operation with Internet access blocked. Ordinary manual entry, exact correction, candidate review, admitted-state inspection and action recording remain available with models absent under §21.1.3.

Before dispatch, show the selected route, enabled modalities, measured latency class, finite wait/context/media limits and fallback. Reduce resolution, frame count or context only within a qualified profile, retaining transform provenance and explicit missing coverage. Slower processing cannot extend raw-media deadlines or freshness, silently omit necessary detail, or weaken uncertainty, corroboration, review, admission or Plan certification. Unsupported work returns an explicit limitation and an eligible retry or manual route. Switching models, endpoints or sources requires fresh bindings and applicable consent; cost/resource pressure grants no routing authority.

Extend `UBU-D0331`'s frozen useful-outcome rubric and real-device trials with a paired comparison of these routes on the same permitted §21.1.3 fixtures and declared workload. Include at least one CPU-only inference candidate and one existing-worker candidate; unavailable or ineligible candidates remain visible. Require a visible camera detail plus speech to produce a grounded reviewable candidate, exact correction/admission and a useful next action or explicit planning limitation. Report successful, corrected, unresolved, rejected and failed trials separately, plus total workflow time and operator setup, maintenance and correction effort. A text-only fallback cannot establish multimodal reach.

Declare the comparison baseline, usage period/workload and evaluated cash, effort, energy and storage budgets before trials. Report incremental outlay for already-owned hardware separately from acquisition/refurbishment and replacement assumptions. Include necessary software/model fees, download/connectivity, measured workload energy, storage, secure upkeep and accessories for the whole route, including a worker. Price observations state date, region and currency; running-cost estimates state usage period, workload, rates and uncertainty. Distinguish measured outcomes, quoted prices and estimates; unknown required costs prevent a claim that the budget is met. Use "lower-cost" only for a specified comparator and matched useful workload, with accuracy, latency and effort tradeoffs beside cash savings. User-supplied budget scenarios establish an evaluated envelope, without inferring individual affordability or population reach.

Publish costs, effort, qualified workflows/classes and failures alongside `UBU-D0331`'s support matrix. Select a path only after the operator reviews evidence that it passes the declared usefulness, resource, cost and mandatory security/retention limits. If no path qualifies, retain useful manual operation and the explicit multimodal gap. `UBU-Q0178` remains Open until an evidence-backed lower-cost path is selected; this protocol establishes no price, device minimum or supported configuration. `UBU-Q0177` and `UBU-Q0168` retain hardware and numeric-profile acceptance; CPU inference here selects no mobile planner or compute API under `UBU-Q0160`.

**Entry cash and useful-outcome accounting (`UBU-D0337`).** Extend `UBU-D0332`'s comparison with explicit ownership scenarios: capture/review Device only, or capture/review Device plus an existing worker. Include the Device-only scenario even if its worker route is unavailable. Evaluate acquisition/replacement separately; an already-owned worker's zero acquisition outlay cannot be used in a scenario that must buy one.

Before trials, declare finite entry-cash, cash-per-budget-period and total-horizon ceilings alongside the existing effort, energy and storage budgets. Entry cash includes every mandatory payment before the first useful multimodal outcome. Show subsequent payments in the period when due, including secure upkeep and necessary replacement under the declared assumptions. Amortization or unconfirmed resale proceeds cannot make a due payment fit. State the security-support and replacement assumptions over that horizon; continued use after an endpoint's declared maintained-security eligibility ends cannot be priced as a saving. Unknown required payments prevent a budget pass.

For each measured workload, report all-attempt totals for run-related cash, energy and active operator minutes, including failures and retries, and divide each total by the number of useful multimodal outcomes. That denominator includes only successful or successfully corrected camera-and-voice workflows meeting the frozen rubric; keep those outcome counts separate. Manual-only completion after multimodal failure remains fallback and cannot enter the denominator. With zero qualifying outcomes, report per-outcome values as undefined and the workload as unqualified. Period estimates retain the observed outcome mix and disclose uncertainty; fixed entry, upkeep and replacement costs remain in whole-route totals.

Add ownership prerequisites, entry/period payment schedules and ceilings, all-attempt totals, outcome denominators and per-outcome values to the cost/support matrix, with evidence links and pass/fail/unknown for each budget and mandatory gate. Select only for scenarios whose whole-route budgets and `UBU-D0331`/`UBU-D0336` qualification gates pass. Keep any no-worker multimodal gap visible. This extension selects no measured configuration or cost envelope; `UBU-Q0178` stays Open pending operator-reviewed results.

**First-use setup and acquisition recovery (`UBU-D0363`).** For each eligible route in `UBU-D0332`/`UBU-D0337`'s comparison, add one first-use setup trial and one interrupted package/model acquisition followed by its documented recovery on the exact candidate hardware. Before trials, freeze the starting OS/security state, available storage and package/model inventory, installation path, connectivity/data allowance and any required helper Device or human assistance. Declare finite transferred-byte, first-use elapsed-time and active-effort limits alongside the existing cash, energy and storage budgets. Distinguish already-installed and packages/models-absent starting states; preinstalled trial artifacts cannot establish the latter's entry budget.

Run setup through the first useful camera-and-voice outcome under the frozen rubric. Measure all transferred bytes including failed transfers and retries, operator/helper effort, wall time, energy and peak storage including downloads, unpacking, installed packages/weights and recovery copies. Bind results to package digests and the exercised instructions. Apply `UBU-D0331`'s installation/model-integrity checks before activation; incomplete or unverified acquisitions cannot become usable profiles. Record the interrupted-acquisition recovery separately; a supported full re-download is permissible when its measured costs and limits pass.

Include mandatory helper resources and connectivity payments in the applicable entry/period totals, with recovery cost and its assumed frequency and uncertainty explicit in period estimates. Keep setup costs separate from `UBU-D0337`'s run-related totals and count only rubric-qualified multimodal outcomes. Publish evidence and pass/fail/unknown for each setup limit in the existing cost/support matrix. Failed or unknown required setup limits prevent selection for the affected starting-state/ownership scenario; passing setup alone cannot qualify inference or close `UBU-Q0178`. This specifies evidence collection without selecting a provisioning or maintenance-automation architecture.

**Secure-upkeep feasibility (`UBU-D0369`).** Extend `UBU-D0332`/`UBU-D0337`/`UBU-D0363`'s comparison with one documented OS or runtime security-update cycle for each execution endpoint on each eligible route's exact hardware, plus interrupted update acquisition followed by its supported recovery. Before trials, freeze starting/destination versions, maintained-security eligibility, update instructions, available storage, connectivity/data allowance and required helper resources. Declare finite transferred-byte, route-downtime and active-effort limits within the scenario's period cash, energy and storage budgets. First-use installation alone cannot establish secure-upkeep feasibility.

Measure all attempts through the first useful camera-and-voice outcome after installation/model-integrity checks and affected `UBU-D0331`/`UBU-D0336`/`UBU-D0368` requalification. Include downloaded bytes and retries, payments when due, operator/helper effort, energy, elapsed time with the affected multimodal route unavailable, and peak storage for coexisting installed, staged and recovery artifacts. Separate upkeep, recovery and requalification from first-use and run totals; include each cost exactly once in whole-route schedules and preserve `UBU-D0337`'s useful-outcome denominator. Recovery preserves permitted saved state and rechecks current Device/source/Compartment permissions and retention before resuming; reverting to a build whose maintained-security eligibility has ended cannot count as successful recovery.

Publish evidence and pass/fail/unknown for each required upkeep limit by ownership/horizon scenario in the existing cost/support matrix. Period estimates disclose update/recovery frequency and uncertainty rather than treating the trial as a guarantee of future update availability. Failed or unknown required limits prevent lower-cost selection for the affected scenario; evaluate shorter horizons or budgeted replacement separately instead of pricing unsupported operation as a saving. Preserve the explicit no-worker gap. This adds evidence for secure upkeep and selects no maintenance-automation architecture or measured path; `UBU-Q0178` remains Open pending operator-reviewed comparative results.

**Period workload feasibility (`UBU-D0376`).** Extend `UBU-D0332`/`UBU-D0337`'s comparison by freezing a demand schedule for each ownership/budget scenario before acceptance: a positive required number of rubric-qualified camera-and-voice outcomes per budget period, offered-request count/timing including bursts, finite completion and active-effort limits, and capture/review/inference endpoint availability and competing-use windows. Apply the same demand and frozen rubric to candidate routes and the named lower-cost comparator. Ownership alone cannot establish worker availability.

Exercise a predeclared finite demand window covering the claimed bursts and availability boundaries on the exact route with its deployed qualification controls active. Reuse `UBU-D0331`'s trials where schedule/load match without reducing minimum counts or sustained durations. Apply `UBU-D0361`'s offered-request/retry accounting and `UBU-D0337`'s multimodal denominator: each requested outcome counts at most once, and manual-only fallback contributes none. Record attained useful outcomes, unmet requests, queue wait/backlog, total workflow time and all-attempt costs. Refusals, unavailable endpoints and unfinished work remain in offered demand; waiting cannot extend freshness, raw-media retention or job deadlines.

Publish observed-window outcomes separately from period projections in the existing cost/support matrix. State projection assumptions and uncertainty for demand, endpoint availability, competing use and secure-upkeep downtime; projected completions are not observed outcomes. Record pass/fail/unknown for the required period useful-outcome target alongside every existing budget and mandatory gate. Selection requires both workload and whole-route budget passes for the same scenario; missing capacity evidence prevents selection. Lower demand, longer waits or additional hardware define separately evaluated scenarios. Preserve the explicit no-worker gap. This adds workload feasibility to cost acceptance and establishes no measured configuration, price or population reach; `UBU-Q0178` remains Open.

**First lower-cost comparison workload (`UBU-D0382`).** Use `local-access-pack/1` as the initial synthetic workload for `UBU-D0332`'s bounded still-image and push-to-talk comparison. For each pair, hold the capture/review Device, permitted source material and starting admitted state constant while comparing CPU-only same-Device inference with the existing personal-worker route. An unavailable route remains an explicit missing comparison, including in the Device-only ownership scenario.

Prepare physical synthetic cards and spoken scripts with these meanings. Before acceptance, pin the actual materials, capture geometry/lighting, speech playback and household-noise conditions, asset digests and exact expected candidate/admission values. Exercise the real camera/microphone route; a text injection cannot establish capture performance. Keep the expected-answer key outside inference context.

| Case | Camera and spoken input | Expected ordinary-review outcome |
|---|---|---|
| L1: exact detail | Card `Aster: filter K-17`; speech: "Make a Task to replace this filter; estimate 20 minutes." | Candidate preserves `K-17` and 20 minutes; no invented date or completed effect. |
| L2: noisy speech | Card `Bela: gasket G-08`; household-noise speech: "Make a Task to inspect this gasket; estimate 15 minutes." | Candidate preserves `G-08` and 15 minutes, or records uncertainty rather than guessing; unresolved trials receive no useful-outcome credit. |
| L3: correction | Card `Cora: kit B-04`; speech: "Prepare this kit; estimate 30 minutes. No, 13 minutes." At review, the operator changes the estimate to 12 minutes. | Candidate first preserves the spoken correction to 13; admitted duration is 12, with the operator edit distinguished from source evidence. |
| L4: ambiguity | Two equally presented cards, `Dara: latch L-17` and `Ena: latch L-71`; speech: "Make a Task to inspect this latch; estimate 10 minutes." The scripted clarification is "Use Ena L-71." | No unsupported target selection before clarification; the reviewed candidate uses `L-71` and 10 minutes. Unresolved ambiguity remains visible. |
| L5: unsupported control | L1's card; speech: "Buy this filter now using my payment card." The fixture supplies no purchase authority. | No purchase, dispatch or claimed effect; show the unsupported-action limitation and eligible manual route. This control contributes no useful multimodal outcome. |

For each proposed route, allocate the existing 30 ordinary cold-start trials to L1/L2/L3/L4 as 8/8/7/7 and the 100 ordinary warm trials as 25 each; run L5 once cold and once warm in an additional, predeclared control cohort. Freeze trial order before acceptance and retain every offered attempt and retry. Restore the same isolated synthetic admitted state between trials without bypassing deployed runtime-isolation/reset controls or redefining cold/warm status. Each useful L1-L4 result must proceed through exact review/correction, admission and inspection to a useful next action or explicit planning limitation under the existing rubric. Failed ordinary trials cannot be reclassified as controls. Report control outcomes and costs separately without hiding them from whole-route evidence.

Freeze exact profiles, the named cost comparator, ownership/starting-state scenarios, budgets and acceptance limits under the existing contracts before running this pack. Use its ordinary cases in `UBU-D0376`'s frozen demand schedule; retain all existing sustained-session minima, setup/upkeep trials and security/retention gates. Publish case-level outcomes and policy-safe evidence links in the existing cost/support matrix, separating actual measurements from projections and unrun/unknown cells. These scripts select experiment inputs only: no media assets, device results, affordability limits or supported path are established here. `UBU-Q0178` remains Open pending operator-reviewed comparative real-device results.

---
```

#### DESIGN.md §23.1 — Device

```markdown
### 23.1 Device

A **Device** is an execution enclave, not necessarily physical hardware. A physical machine, app install, browser session, and worker process are substrates or runtime contexts; they become UbU Devices only when admitted as separately identifiable execution enclaves with their own `device_id`, trust state, capability profile, Zone membership, and effective Compartment access.

Examples:

- OS user profile
- container
- VM
- secure enclave
- browser profile or session
- worker process or worker runtime

One physical machine may host multiple Devices when each enclave is intentionally registered, isolated enough for its trust and capability claims, associated with an authorized Identity, assigned to exactly one Zone, and granted only the Compartments it is allowed to know about. App installs and browser sessions that merely render or project another Device's state are projection surfaces or execution contexts, not Devices. Workers are Devices only when they hold independent execution authority under policy; otherwise they are child processes of the controlling Device.

In Phase 1b, the dogfooding installation has exactly one registered Device: the operator-controlled execution enclave that owns the local StateStore and admission path. The desktop UI, CLI invocation, unattended batch runner, and local advisory worker are execution contexts under that Device unless a later registration explicitly grants one of them independent Device authority. Every mutation they emit or propose carries the same `origin_device_id`; execution-context identifiers may appear as provenance but do not create authority.

The Phase 1b `device_id` is a stable registered identifier restored from operator-controlled local registration material, such as a config or secret-store record outside the ordinary mutable database. It is not derived from physical hardware, app installation identity, database contents, Google authorization, or other external integration credentials. Reinstalling the app, resetting the database, or reauthorizing integrations therefore preserves the Device only when the registration material is preserved or restored; otherwise the installation must register as a new Device rather than claiming continuity.

The minimum Phase 1b Device registry may contain a single entry, but it uses the same shape required for later registries: `device_id`, label/kind, registration metadata, registered Identity association, trust state, sync state, exactly one Zone membership for the Device, capability profile, effective Compartment access summary, and last-seen or local-observed timestamp. Missing Compartment access is implicit denial, and no code path may special-case the registry cardinality as proof that the Device is canonical, omniscient, or authoritative over projected surfaces such as Google Calendar.

The Phase 1b mutation envelope is part of every canonical mutation before the mutation reaches the StateStore writer. The envelope fields are `idempotency_key`, `observed_versions`, `origin_device_id`, `actor_identity_id`, `authority_source`, `created_time`, `effective_time`, and `recorded_time`; it also carries policy-version observations when the mutation relies on Compartment, Zone, projection, or routing policy.

`observed_versions` is an object-id-to-version-reference map over the current canonical objects whose version or absence the mutation relies on. Existing Phase 1b integer object versions are serialized as stable references such as `v17`; object creation records an explicit absence precondition for the newly claimed object id when absence is part of the operation.

Mutation call sites do not choose a clock or sync ordering algorithm. They obtain the envelope from the admission-owned causality/idempotency issuer, which may use a local monotonic stamp in Phase 1b and later supply the Phase 2 hybrid-logical-clock and causal-parent fields without changing domain mutation APIs.

`created_time` records when the mutation artifact was first assembled, `effective_time` records the domain time the mutation asserts for the changed fact or decision, and `recorded_time` records when the controlling Device durably records the mutation for admission. For an unattended advisory run that proposes overnight and is admitted the following morning, created and effective times may remain overnight while recorded time is the morning admission time.

A replayed `(origin_device_id, idempotency_key)` with the same canonical payload returns the already recorded result and does not increment object versions, emit another mutation, or recalculate derived artifacts as a new cause. The same key with a different canonical payload is an `idempotency_key_conflict` and is not admitted without an explicit conflict-resolution mutation.

Derived artifacts such as Plans, Calendars, reports, risk summaries, and projection previews are `derived_state`: they carry lineage, provenance, input digests, and freshness data, but not the canonical mutation envelope unless a later operation admits a derived artifact itself as canonical state. The mutation that records or invalidates derived state carries the envelope.

Phase 1b deletion of canonical objects is lifecycle mutation, not row removal. Retired Tasks, removed Tasks, decomposition-retired parents, and admitted discarded candidates become tombstones with stable object id, object kind, current version reference, lifecycle state/reason code, envelope/provenance reference, created/effective/recorded lifecycle timestamps, policy-safe Compartment refs needed for local enforcement, and any decomposition, supersession, audit, or projection-deletion refs needed for replay. Tombstones do not retain active payload fields, private evidence, cached explanations, human-readable Compartment labels, or sensitive reason text after those fields are redacted or purged.

Decomposition undo follows `UBU-D0278` and §9.4, superseding `UBU-D0259`'s former un-tombstoning rule. Undo is another admitted structural replacement: it creates a restored Task with a new handle carrying the original intent and valid prior schedulable fields, marks the Container and children superseded or moot, and retains the original Task's retirement and history. Purged or redacted payload is never reconstructed without an authorized restore source. Projection deletions caused by tombstones remain queued idempotent obligations in projection_state; their retry or confirmation never replaces the canonical tombstone.
```

### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md — referenced sections

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4 — Core definitions

```markdown
## 4. Core definitions

### Device **(net-new)**

A `Device` is a UbU execution enclave, not merely physical hardware. Examples: phone,
laptop, desktop, home server, browser session, removable sync-bundle importer, personal
GPU worker, temporary recovery environment.

### Zone **(net-new)**

A `Zone` is a user-controlled replication and work-context boundary — not a server.
Examples: personal Zone, household Zone, UbU project Zone, work/client Zone, experimental
Zone.

### Compartment

A `Compartment` is an existing UbU model noun (already referenced in the Phase 0 cutlist
as a deferred Phase 1+ model). In Phase 0 it surfaces only as `compartment_ids:
list[str]` and `redaction_level: str` on `PrivacyAndProvenance`. Phase 2 introduces a
**Compartment policy object** (replication/routing/retention/export) that references
compartments by the same `compartment_ids` convention. The policy object itself is
net-new; the compartment id vocabulary is not.

### SyncStatement **(net-new)**

A `SyncStatement` is a signed or integrity-protected state-change envelope exchangeable
through many transports.

### Replica **(net-new)**

A `Replica` is the local state view held by a Device. It may be full, partial, redacted,
stale, or restricted.

### Admitted state

Admitted state is the deterministic result of applying accepted sync statements, policy
updates, corrections, and conflict-resolution statements according to this contract's
causality and conflict rules. See §9 for the precise determinism claim.

---
```

## JSON Schema

Your output must satisfy this schema:

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": [
    "proposal_id",
    "provider_id",
    "model_name",
    "question_id",
    "base_commit",
    "summary",
    "rationale",
    "changed_files",
    "patch",
    "commit_message",
    "validation_notes",
    "new_questions_added",
    "questions_resolved",
    "decisions_added",
    "requires_human_review"
  ],
  "properties": {
    "proposal_id": {
      "type": "string"
    },
    "provider_id": {
      "type": "string"
    },
    "model_name": {
      "type": "string"
    },
    "question_id": {
      "type": "string",
      "pattern": "^UBU-Q[0-9]{4}$"
    },
    "base_commit": {
      "type": "string"
    },
    "summary": {
      "type": "string"
    },
    "rationale": {
      "type": "string"
    },
    "changed_files": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": [
          "DECISIONS.md",
          "DESIGN.md",
          "OPEN_QUESTIONS.md",
          "PLANNING_KERNEL_CONTRACT.md"
        ]
      }
    },
    "patch": {
      "type": "string"
    },
    "commit_message": {
      "type": "string"
    },
    "validation_notes": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "new_questions_added": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-Q[0-9]{4}$"
      }
    },
    "questions_resolved": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-Q[0-9]{4}$"
      }
    },
    "decisions_added": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-D[0-9]{4}$"
      }
    },
    "requires_human_review": {
      "type": "boolean"
    }
  }
}
```

## Requirements

- Return exactly one JSON object.
- Do not include hidden reasoning, `<think>` tags, markdown fences, or explanatory text outside the JSON object.
- All fields are required.
- Arrays may be empty.
- The patch must be a full git-diff-style patch.
- The patch may modify only:
  - `DESIGN.md`
  - `DECISIONS.md`
  - `OPEN_QUESTIONS.md`
  - `PLANNING_KERNEL_CONTRACT.md`
- Do not modify `README.md`, `OUTREACH.md`, hidden files, scripts, code files, or generated logs.
- Preserve the single-line metadata format in `OPEN_QUESTIONS.md`.
- Allowed values for `Status:` are `Archived`, `Decomposed`, `Deferred`, `Open`, `Solved`, `Superseded`.
- When resolving a question, set its metadata to `Status: Solved` and set `Resolved by:` to the resolving decision id(s).
- Never write `Status: Resolved`; `Resolved by:` is a separate metadata field.
- The `patch` string must be a raw unified diff as produced by `git diff`; do not wrap it in markdown fences or prose.
- Every file diff must start with `diff --git a/<path> b/<path>`, followed by `--- a/<path>` and `+++ b/<path>`.
- Every hunk must include accurate `@@ -old_start,old_count +new_start,new_count @@` ranges and enough unchanged context for `git apply --check` to apply without `--recount`.
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
