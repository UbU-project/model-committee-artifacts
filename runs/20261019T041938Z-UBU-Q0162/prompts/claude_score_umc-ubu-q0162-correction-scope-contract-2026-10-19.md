# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `anthropic`

## Selected question

Question ID: `UBU-Q0162`  
Question title: `Enforce append-only Log correction rather than in-place editing`  
Base commit: `0f700ab4cf63da706da5edeff3f609ab14eafc4b`

```markdown
## UBU-Q0162: Enforce append-only Log correction rather than in-place editing

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-19 Scored from commit: None

### Question

Where does the operator-reported editable Log violate the existing append-only contract, and what minimum implementation correction closes it?

### Current direction

Append-only Logs and linked corrections are already design rules under `UBU-D0071`, not a newly adopted requirement. Audit edit paths and distinguish annotation, corrected query views and compensating entries from changing admitted history. Preserve original assertions, correction/revocation links and privacy-minimized media-deletion metadata. Local source audits identify a generic `LogEntry` admission gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation, incomplete Log replay comparison at the shared Log writer, missing linked-history discovery and consumers of uncorrected history in routine-duration planning, completion reports and reopen actions; the reported edit path, filed remediation ticket and passing implementation evidence remain outstanding. This documentation ticket changes no store or UI behavior.

### Audit and minimum repair

Record a privacy-safe reproduction identifying the running repository/revision, reported control or command, affected entry reference, and admitted record before and after the operation. Trace that operation through its UI/CLI, handler, admission and store writer; inventory other application-owned update, delete, replacement, import and replay paths that can reach the same records. Attach actual file/symbol/route references to the remediation ticket. An editable draft, changed corrected view, rebuilt index or reserialized storage file alone is not evidence that admitted history changed.

The minimum repair uses the existing Log envelope and event types:

- Route added notes through `log_annotation_added` with `annotation_of`; route withdrawal or replacement of an assertion through `log_correction_added` with `correction_of`. Each is a separately admitted entry with a fresh `log_entry_id`, correcting actor/Device, provenance, admission time and idempotency key. Annotation or semantic correction keeps the original envelope, assertion and existing links unchanged. Generic edit/delete paths must reject attempts to change admitted history, including its notes and provenance; an operator's edit authority does not waive this invariant.
- Resolve current query views through admitted correction/revocation links while retaining policy-permitted historical views. A replacement Snapshot creates a new Snapshot and links it under DESIGN.md §12.1. Correcting a Log assertion does not automatically undo Task effects, UniverseState mutations or external actions; any compensating transition needs its own ordinary admission and authority.
- Reuse current authority, policy, observed-version and idempotency checks (`UBU-D0258`). Commit the new entry, admission outcome and any required canonical compensation atomically; invalidate affected derived state through ordinary recalculation. Stale or competing corrections use the ordinary conflict rules (`UBU-D0248`), not an unconditional overwrite or timestamp-only choice.
- Keep policy-authorized media deletion, redaction and purge distinct from semantic correction. Preserve only permitted minimized correction/revocation/deletion links and outcome metadata under DESIGN.md §12.2 and the existing Compartment rules. Neither originals nor correction payloads, indexes or derived views may retain or reconstruct content whose policy requires removal. Deletion must not fabricate replacement evidence or imply that an unconfirmed remote purge completed.

### Source audit and remediation handoff

A 2026-10-12 static source audit of the clean local `ubu-store` checkout at `83a099f55220b1286ffb2dee3c9d19006e2090b5` identifies an alternate mutable representation of `LogEntry`:

- `src/admission.rs::object_type_from_str` accepts `LogEntry`, and `validate_object_record` permits it as a `NewObjectRecord`. `src/queries.rs::admit_object` reaches `admit_prepared_object` and `write_object_row`, whose `VersionRef::Version` branch updates `objects` without a `LogEntry` guard. `admit_batch` with `BatchWrite::Object` reaches the same writer.
- The source of `tests/append_log.rs::cross_writer_key_is_global_per_device` deliberately admits a `LogEntry` into `objects`. The dedicated `src/queries.rs::append_log_entry` and `append_prepared_log` instead insert into `logs`. This source audit does not establish which path the operator's reported control reaches or demonstrate a changed admitted record at runtime.

A 2026-10-13 follow-up static audit of the same clean `ubu-store` revision also identifies `src/candidates.rs::admit_advisory_candidate`: it accepts a `NewObjectRecord` and calls `src/queries.rs::admit_prepared_object`, reaching `write_object_row` before `src/candidates.rs::write_decision` records candidate acceptance. There is no `LogEntry`-specific guard on this route. Guarding only the public `admit_object` wrapper or batch dispatch would leave candidate acceptance able to reach the generic create/update gap. This is another source-level reachability finding, not a reproduction of the reported control.

A 2026-10-14 static audit of the same clean `ubu-store` revision identifies a related legacy identity-collision path: `migrations/0001_initial.sql` declares separate primary keys for `objects.id` and `logs.id`; `src/queries.rs::append_prepared_log` inserts into `logs` without checking for an existing `objects` row with that id. Log append requires no absence precondition for its own id. The replay-target check in `prepare_mutation` and `tests/append_log.rs::cross_writer_key_is_global_per_device` cover reused keys, leaving a fresh key able to claim a legacy `LogEntry` id in `logs`. This is a source-level finding about identity reuse, not a runtime reproduction of the reported edit.

A 2026-10-14 static audit of the same clean `ubu-store` revision identifies missing correction-link validation: `src/models/log_record.rs::NewLogRecord` accepts a string `event_type` and arbitrary JSON `payload`; `src/queries.rs::append_prepared_log` validates the Log id, provenance and timestamp, but applies event-specific validation only to `recalculation_requested`. It does not validate `annotation_of` or `correction_of` before inserting into `logs`. Direct Log append and `BatchWrite::Log` share this path. Blocking generic `LogEntry` writes therefore leaves linked-correction validation outstanding. This is a source-level finding, not a reproduction of the reported edit or a passing behavioral result.

A 2026-10-15 static audit of the same clean `ubu-store` revision identifies incomplete Log replay comparison: `src/queries.rs::append_log_entry` and the `BatchWrite::Log` branch of `admit_batch` pass only `record.payload` to `prepare_mutation`. With that payload unchanged, `append_prepared_log` returns the recorded Log result before comparing the requested `id`, `event_type`, `object_refs`, `provenance` or `created_at`. A changed request can therefore be reported as a successful replay of a different immutable record. This does not rewrite the original; it can falsely acknowledge a requested annotation or correction. The inspected `tests/append_log.rs::log_empty_preconditions_admit_and_replay_without_writes` and `log_changed_payload_conflicts_without_changes` cover exact replay and changed payloads, not these unchanged-payload changes. This is static evidence, not a runtime reproduction or passing check.

A 2026-10-15 read-side static audit of the same clean `ubu-store` revision identifies missing linked-history discovery: `src/queries.rs::get_object_history` searches only `object_refs_json` with SQL `LIKE` and orders by `created_at`; `src/replay.rs::replay_state` delegates directly to it. A correction referencing only the original Log entry, with `correction_of` in its payload, is omitted when querying the original domain object unless its id is repeated in the correction's `object_refs`. Neither function traverses correction/annotation links or resolves corrected meaning. `tests/replay_state.rs::replays_object_history_from_logs` appends one `object_admitted` entry and checks the history length; it does not cover linked corrections. Raw historical enumeration remains legitimate, but these functions alone do not establish corrected query semantics. This is static source evidence, not proof of downstream misuse or a reproduction of the reported edit.

A 2026-10-18 static audit of the clean local `ubu-orchestrator` checkout at `9fddef16990ac89d07a168c75bf04b77c35b9bc3` identifies a direct planning consumer outside those store history functions. `src/services/planning_service.rs::observed_routine_events` joins `logs` to routine Tasks using `object_refs_json[0]`, selecting start/completion events without following annotation/correction links. `build_request_from_store_with_context` feeds those rows to `src/services/duration_model.rs::by_group` and `derive`, then substitutes the derived estimate into planner Task inputs. A linked correction/revocation is excluded by the event filter while the original remains eligible; repairing `get_object_history` or `replay_state` alone cannot repair this consumer. `tests/duration_models.rs` covers observed estimates and legacy event names but contains no linked-correction fixtures. This is static source evidence, not a reproduction of the reported edit or a passing planning result.

A 2026-10-18 follow-up static audit of the same clean `ubu-orchestrator` revision identifies `src/services/calendar_interaction.rs::latest_completion`: it selects raw `task_done` or completion `decision_recorded` entries by Task reference and descending `rowid`, without resolving correction links. `src/services/report_service.rs::time_by_category` uses its duration evidence; `src/services/log_service.rs::reopen_completion` and `reopen_calendar_completion` use its completion identity. The same `log_service.rs::record_task_action_with_calendar` uses its presence to suppress repeated Task effects. Filtering this helper through corrected history without preserving already admitted effect outcomes could therefore permit a second effect application after withdrawal hides the earlier completion. `tests/task_reopen.rs::effects_apply_once_per_task_across_an_undo_and_the_second_completion_says_so` covers ordinary reopen/recomplete, without linked corrections. This is static source evidence and a repair hazard, not a reproduction of the reported edit or a passing behavioral result.

Proposed implementation remediation ticket: **Enforce Log-only admission for `LogEntry` and append linked corrections** in `ubu-store`; file it and link it to `UBU-Q0162`. Reject generic `LogEntry` creates and updates at the shared admission boundary, including `BatchWrite::Object` and `src/candidates.rs::admit_advisory_candidate`, while retaining `LogEntry` in the ID registry. Use Log admission for fresh entries and linked annotations/corrections as specified above. At the shared Log writer `src/queries.rs::append_prepared_log`, reject a fresh append whose id is already held by a legacy `objects` row, including `BatchWrite::Log`, within the admission transaction and independently of the origin Device or idempotency key; preserve ordinary successful Log replay. Inventory any legacy `objects` rows of this type and any ids shared by `objects` and `logs` before repair; preserve policy-permitted originals and links, and surface conflicting identities through ordinary review rather than overwriting or silently dropping history. Attach the running-revision reproduction and UI/CLI-to-writer trace when established.

The same remediation ticket must validate `log_annotation_added`/`annotation_of` and `log_correction_added`/`correction_of` at `src/queries.rs::append_prepared_log`, within the admission transaction and before a new entry or receipt is written. Require the applicable link to identify a distinct admitted original under current authority and visibility policy. Drafts and advisory candidates cannot establish a Log target; unresolved legacy identity collisions use ordinary review. Missing required imported history remains staged under existing dependency rules. Policy-permitted minimized target metadata may establish a link after authorized payload removal; validation must not retrieve or reconstruct purged content.

At the same shared `append_prepared_log` boundary, compare the complete immutable Log request with the admitted result before returning replay success: `id`, `event_type`, `object_refs`, `payload`, `provenance` and `created_at`, including annotation/correction links wherever represented. Use canonical JSON comparison for JSON fields. Reuse of the Device-global key with different immutable content must return `idempotency_key_conflict`; exact permitted replay returns the original result without writes. Preserve other writers' replay behavior. Existing payload-only receipts require comparison with their policy-permitted admitted Log record, without rewriting history or receipts. If retained metadata cannot establish equality after authorized redaction/purge, withhold replay success and surface the limitation through ordinary review; neither reconstruct purged content nor retain a denied comparison copy.

Include the read side in the same remediation ticket at `src/queries.rs::get_object_history` and `src/replay.rs::replay_state`: discover history through exact structured object references and admitted annotation/correction links, including chains whose later entries reference only earlier Log ids. Preserve policy-permitted historical assertions and expose corrected interpretation separately under `UBU-D0071`; an annotation adds context without withdrawing the assertion. Resolve replacement/revocation through admitted links and ordinary conflict rules, never `created_at` ordering alone. Identify any callers using raw history as current truth and route them through the corrected view before claiming planning/query conformance. Apply current visibility, retention and dependency rules to link traversal; missing required history leaves the view explicitly incomplete within permitted visibility, and purged content cannot be fetched or reconstructed.

Include `ubu-orchestrator`'s `observed_routine_events` and `build_request_from_store_with_context` in the same remediation handoff. Resolve admitted links before `duration_model::by_group` pairs events or `derive` estimates durations; retain source Log identities through resolution. An annotation or friction-only correction cannot change numeric duration evidence. Extend `tests/duration_models.rs` with linked annotations, numeric-evidence replacements, withdrawals, chained corrections and competing corrections referencing only prior Log ids. Assert the actual planner request uses only uncontested, policy-permitted corrected evidence, counts each observation once and preserves the declared/admitted duration model when usable observations fall below the existing minimum. Include otherwise sufficient history with missing required dependencies or unavailable purged evidence; neither can silently become a complete observed-duration model. Direct/batch admission and exact replay must yield equivalent evidence and observation counts. Preserve originals and prior links; view resolution emits no correction, compensation or receipt and cannot reconstruct purged content. Existing review/admission requirements for learned model changes still apply. These are required checks, not passing results.

Include `calendar_interaction::latest_completion` and these callers in the same remediation handoff. Resolve admitted links for report and interaction interpretation, preserving source completion identities and correction scope; annotation or friction-only correction cannot withdraw completion or change duration evidence. Recheck the corrected target, current Task version and authority at explicit reopen admission; competing or incomplete targets follow ordinary conflict/dependency rules. Keep already admitted effect outcomes distinct from corrected assertion eligibility, using ordinary admission/idempotency records. Withdrawal alone cannot reopen a Task, reverse UniverseState, clear an applied-effect outcome or authorize a second application. If policy-permitted history or outcome metadata cannot establish safe action after purge or missing dependencies, withhold that action through ordinary review without reconstructing removed content.

Extend `ubu-orchestrator`'s `tests/reports.rs`, `tests/task_reopen.rs` and `tests/calendar_interaction.rs` with admitted annotations, scoped duration replacements, withdrawals and chained/competing corrections referencing only prior Log ids. Assert actual report totals and app/Calendar completion-target checks respect corrected meaning without silently choosing a competing or withdrawn assertion. With an incrementing Task effect, complete once, explicitly reopen, withdraw the prior completion assertion, then complete again: the counter and UniverseState version must not increment a second time. In correction-only fixtures, admission and repeated view resolution must leave Task/UniverseState state and prior Logs unchanged and emit no compensation; reads create no Log or receipt. Exercise equivalent direct/batch correction admission and exact replay, restart, missing required history and authorized payload purge; unresolved safe-action checks must preserve saved state and denied content must remain unavailable. These are required checks, not passing results.

Extend `tests/admit_object.rs`, `tests/admit_batch.rs` and `tests/append_log.rs` to reject generic creates and valid-version updates of synthetic legacy `LogEntry` records, reject a batch containing the bypass without partial records or admission receipts, and prove originals survive linked correction and replay. Remove the cross-writer fixture's reliance on admitting `LogEntry` through ordinary-object admission while retaining Device-global idempotency coverage. These checks supplement the full closure evidence below; no implementation checks have been run by this documentation audit.

Extend `tests/append_log.rs` and `tests/admit_batch.rs` with a synthetic legacy `objects` row and a Log append claiming its id under a fresh key, with no target-absence precondition; exercise both the same origin Device and a different origin Device. Rejection must preserve the original row and existing receipts, create no Log or mutation receipt, and roll back any earlier valid writes in the batch. Keep fresh-id Log append, linked correction and successful replay coverage passing. These are required checks, not passing results.

Extend `tests/advisory_candidates.rs` with otherwise valid reviewable candidates submitting a generic `LogEntry` create and a valid-version update of a synthetic legacy `LogEntry`. Rejection must preserve the original record and the candidate's payload, version, lifecycle and admission links, with no new object/Log row, admitted candidate decision or mutation receipt. Existing decision events and receipts remain unchanged, and the candidate remains reviewable. Keep ordinary candidate-to-object admission coverage passing. These are required checks, not passing results.

Extend `tests/append_log.rs` and `tests/admit_batch.rs` with otherwise valid annotation and correction requests missing their applicable link, using a wrong-kind reference, referencing an unadmitted entry or pointing to the new entry itself. Rejection must preserve originals, prior links and existing receipts, create no Log or mutation receipt, and roll back earlier valid batch writes. Also prove valid target-then-correction admission within one batch, links to permitted minimized metadata after media deletion, and exact replay without duplicate entries. These are required checks, not passing results.

Extend `tests/append_log.rs` and `tests/admit_batch.rs` with otherwise valid retries preserving the Device/key and payload while changing each of `id`, `event_type`, `object_refs`, `provenance` and `created_at` independently. Each must conflict, preserve originals, prior links and existing receipts, create no new Log or receipt, and roll back earlier valid batch writes. Include linked annotation/correction fixtures, changed-link conflicts and exact replay controls, plus legacy payload-only receipts and unavailable comparison content after authorized purge. Exact permitted replay must return the original entry without duplicate corrections or writes; unavailable content cannot be reconstructed to decide equality. These are required checks, not passing results.

Extend `tests/replay_state.rs` with an original referencing a domain object, annotation/correction entries referencing only the original Log id, and a later correction linked to a correction. Prove the permitted history includes those entries with originals and prior links unchanged; the corrected view preserves an annotated assertion, applies uncontested replacement/withdrawal and leaves competing corrections subject to ordinary conflict review rather than choosing the latest timestamp. Include exact-reference negative fixtures, equivalent direct/batch admission and retry results, missing required history and permitted minimized links after authorized payload purge. Reads and repeated view resolution must create no Log, mutation receipt or compensation and must not reconstruct denied content. These are required checks, not passing results.

### Closure evidence

Close this question only with a revision-pinned reproduction, an implementation remediation ticket naming the affected paths, and passing behavioral checks showing:

- Annotation, correction and revocation leave the admitted original and prior links unchanged, create the appropriate linked entries, and expose both corrected meaning and permitted historical assertions. Snapshot replacement and required compensation remain explicit separate records.
- Application-owned direct and generic edit/delete bypasses fail; duplicate retry/replay adds no second entry or repeated compensation; stale, unauthorized and conflicting requests cannot rewrite history. Interrupted admission leaves no partial correction/compensation result.
- Query, import/replay and relevant planning inputs respect the admitted links; authorized media deletion leaves evidence unavailable with policy-safe metadata and no recoverable denied copy in dependent views.

### Resolution

Open. Append-only correction is already decided. Static source audits identify a generic `LogEntry` admission/update gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation, incomplete Log replay comparison at the shared Log writer, missing linked-history discovery and consumers of uncorrected history in routine-duration planning, completion reports and reopen actions. The proposed remediation includes shared-boundary guards, fresh-key legacy identity-collision checks, candidate rollback checks, linked-entry validation, complete immutable-record replay checks, link-aware history/corrected-view checks, routine-duration consumer checks and checks distinguishing corrected completion evidence from applied effects. The reported operation's revision-pinned reproduction, filed remediation ticket and passing behavioral evidence remain outstanding; this documentation patch changes no store or UI behavior.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "umc-ubu-q0162-correction-scope-contract-2026-10-19",
    "provider_id": "anthropic",
    "model_name": "claude-fable-5-1",
    "question_id": "UBU-Q0162",
    "base_commit": "0f700ab4cf63da706da5edeff3f609ab14eafc4b",
    "summary": "Add a minimum declared-scope contract for `log_correction_added` to UBU-Q0162's minimum repair: `correction_of` plus `correction_kind` (`withdraw`/`replace`) and `corrected_paths`, with deterministic corrected-view semantics, correction-of-correction chains, competing-correction handling via ordinary review, rejection rules at `append_prepared_log`, and no-rewrite treatment of already admitted unscoped corrections. Updates the Resolution to list the contract among the proposed remediation items and to mark it as awaiting operator approval. Question stays Open; no metadata, decision or other question changes.",
    "rationale": "The question is partially resolved: the append-only rule is decided (UBU-D0071) and the implementation gaps are documented at length, so the right move is to clarify rather than resolve. Reading the handoff as written exposes one data-model gap that no amount of implementation evidence can close on its own: it requires consumers (`observed_routine_events`, `latest_completion`, `get_object_history`/`replay_state`) to honor \"correction scope\", \"withdrawal\", \"replacement\", \"friction-only\" and \"competing\" corrections, yet UBU-D0071 defines only annotation/correction links with optional old/new values, and the recorded 2026-10-14 audit found no event-specific validation for these events. Without a declared, validated scope, a consumer cannot distinguish a duration correction from a withdrawal of the completion it references, and each corrected view would reinterpret free-form payloads differently, making the required planner/report/reopen fixtures untestable. The addition stays within the existing envelope and event types, keeps corrections append-only (legacy unscoped entries are never rewritten), routes overlapping competing corrections to ordinary review per UBU-D0248 rather than timestamp or storage order, and keeps interpretation separate from effect eligibility and compensation. It is placed in the question body in the same style as the existing minimum-repair specification because the question requires human approval; no decision is recorded until the operator approves the data-model choice.",
    "changed_files": [
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -4083,5 +4083,15 @@\n - Keep policy-authorized media deletion, redaction and purge distinct from semantic correction. Preserve only permitted minimized correction/revocation/deletion links and outcome metadata under DESIGN.md \u00a712.2 and the existing Compartment rules. Neither originals nor correction payloads, indexes or derived views may retain or reconstruct content whose policy requires removal. Deletion must not fabricate replacement evidence or imply that an unconfirmed remote purge completed.\n \n+The handoff below relies on correction scope, withdrawal, replacement, friction-only corrections and competing corrections, but `UBU-D0071` names only annotation/correction links with optional old/new values, and the 2026-10-14 audit found `append_prepared_log` applying event-specific validation only to `recalculation_requested`. Without a declared scope no consumer can distinguish a duration correction from withdrawal of the completion it refers to, and each corrected view below would reinterpret free-form payloads differently. The minimum `log_correction_added` contract for this repair is:\n+\n+- `correction_of` identifies exactly one admitted Log entry, either an original assertion or an earlier correction. `correction_kind` is `withdraw` or `replace`. `corrected_paths` lists JSON Pointer paths within the target's `event_payload` or naming its `result` or `effective_at` field; `withdraw` may instead declare the whole assertion. `replace` carries exactly one replacement value per listed path and nothing else. Old values, reason and notes remain optional audit context, not the mechanism. `log_annotation_added` carries `annotation_of`, notes and optional confidence and declares no scope.\n+- A corrected view starts from the admitted assertion. A whole-assertion `withdraw` removes the target from corrected views while permitted historical views keep it; a `replace` substitutes only the listed paths and every unlisted path keeps its admitted value; an annotation changes no meaning. A correction whose paths exclude duration-bearing fields cannot change duration evidence, and one whose paths exclude lifecycle or `result` fields cannot withdraw a completion; friction-only means paths limited to friction, notes or reason fields. Snapshot replacement keeps DESIGN.md \u00a712.1's form: the correction withdraws or narrows the original observation and links the new Snapshot's own admitted entry instead of embedding replacement state.\n+- A correction targeting an earlier correction corrects that correction. Withdrawing it removes its effect and leaves the target's other admitted corrections in force; replacing it supersedes its values for the paths it lists. Because link validation requires a distinct already admitted target, including one admitted earlier in the same batch, chains are acyclic and resolve by following admitted links outward from the original.\n+- Two or more admitted, non-withdrawn corrections of one target with overlapping paths, neither targeting the other, are competing. The corrected view marks those paths contested and leaves them to ordinary review under `UBU-D0248`'s incompatible-intent class; `created_at`, `recorded_at`, storage order and actor rank decide nothing. Corrections with disjoint paths compose deterministically. Contested paths supply no planning, report or completion-target evidence until resolved.\n+- Reject at `append_prepared_log`, with the same no-write and rollback guarantees as a missing link, any correction lacking `correction_kind`, any `replace` without paths or values, any path the target's event type does not define and any value for an undeclared path. Inventory existing emitters of `log_correction_added` before enforcing. An already admitted correction without a declared scope is never rewritten; corrected views treat it as an unresolved whole-target correction for ordinary review rather than inferring withdrawal or replacement. Scope declares interpretation only: it changes no effect eligibility, authorizes no compensation and mutates no UniverseState; the effect/assertion separation above still governs.\n+\n+Add fixtures for each rejection and for whole-assertion withdrawal, path-scoped replacement, friction-only correction, corrected corrections and disjoint versus overlapping competing corrections to the `ubu-store` and `ubu-orchestrator` checks required below, asserting the corrected view, the permitted historical view and the actual planner, report and completion-target inputs separately. These are required checks, not passing results; this contract is a data-model proposal awaiting operator approval and changes no store, planner or UI behavior.\n+\n ### Source audit and remediation handoff\n \n A 2026-10-12 static source audit of the clean local `ubu-store` checkout at `83a099f55220b1286ffb2dee3c9d19006e2090b5` identifies an alternate mutable representation of `LogEntry`:\n@@ -4137,5 +4147,5 @@\n ### Resolution\n \n-Open. Append-only correction is already decided. Static source audits identify a generic `LogEntry` admission/update gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation, incomplete Log replay comparison at the shared Log writer, missing linked-history discovery and consumers of uncorrected history in routine-duration planning, completion reports and reopen actions. The proposed remediation includes shared-boundary guards, fresh-key legacy identity-collision checks, candidate rollback checks, linked-entry validation, complete immutable-record replay checks, link-aware history/corrected-view checks, routine-duration consumer checks and checks distinguishing corrected completion evidence from applied effects. The reported operation's revision-pinned reproduction, filed remediation ticket and passing behavioral evidence remain outstanding; this documentation patch changes no store or UI behavior.\n+Open. Append-only correction is already decided. Static source audits identify a generic `LogEntry` admission/update gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation, incomplete Log replay comparison at the shared Log writer, missing linked-history discovery and consumers of uncorrected history in routine-duration planning, completion reports and reopen actions; the specification itself left correction scope undefined. The proposed remediation includes shared-boundary guards, fresh-key legacy identity-collision checks, candidate rollback checks, linked-entry validation, a declared `correction_kind`/`corrected_paths` contract for `log_correction_added` with deterministic corrected-view, chain and competing-correction rules, complete immutable-record replay checks, link-aware history/corrected-view checks, routine-duration consumer checks and checks distinguishing corrected completion evidence from applied effects. The correction-scope contract awaits operator approval as a data-model choice; the reported operation's revision-pinned reproduction, filed remediation ticket and passing behavioral evidence remain outstanding, and this documentation patch changes no store or UI behavior.\n \n ---\n",
    "commit_message": "UMC: docs: define correction scope contract for UBU-Q0162 Log corrections",
    "validation_notes": [
      "No shell or file-read tool was available in this session, so `git apply --check` was not run; run it against the working tree before committing.",
      "All removed and context lines were copied verbatim from the working-tree excerpt of UBU-Q0162 supplied with the request, which already reflects the uncommitted `Last scored: 2026-10-19` metadata state; the metadata line itself is untouched.",
      "Hunk start line numbers are estimates; each hunk's preimage contains a line unique to UBU-Q0162 (the fourth minimum-repair bullet and the Resolution paragraph), so git apply should locate them by context with an offset. Hunk line counts are exact (5 -> 15 and 5 -> 5).",
      "No new source audit was performed. The added content is a specification gap derived from the question text and the recorded 2026-10-14 audit; it makes no claims about ubu-store or ubu-orchestrator code beyond what the question already records.",
      "Status remains Open; no decision, DESIGN.md section or other question is changed. The correction-scope contract is a data-model proposal requiring operator approval before it is recorded as a decision or implemented."
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
    "proposal_id": "umc-ubu-q0162-correction-scope-contract-2026-10-19",
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
