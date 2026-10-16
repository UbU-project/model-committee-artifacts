# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0162`  
Question title: `Enforce append-only Log correction rather than in-place editing`  
Base commit: `dce1b35c49756da059c6078d136bc93e446e95da`

```markdown
## UBU-Q0162: Enforce append-only Log correction rather than in-place editing

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

Where does the operator-reported editable Log violate the existing append-only contract, and what minimum implementation correction closes it?

### Current direction

Append-only Logs and linked corrections are already design rules under `UBU-D0071`, not a newly adopted requirement. Audit edit paths and distinguish annotation, corrected query views and compensating entries from changing admitted history. Preserve original assertions, correction/revocation links and privacy-minimized media-deletion metadata. Local source audits identify a generic `LogEntry` admission gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation and incomplete Log replay comparison at the shared Log writer; the reported edit path, filed remediation ticket and passing implementation evidence remain outstanding. This documentation ticket changes no store or UI behavior.

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

Proposed implementation remediation ticket: **Enforce Log-only admission for `LogEntry` and append linked corrections** in `ubu-store`; file it and link it to `UBU-Q0162`. Reject generic `LogEntry` creates and updates at the shared admission boundary, including `BatchWrite::Object` and `src/candidates.rs::admit_advisory_candidate`, while retaining `LogEntry` in the ID registry. Use Log admission for fresh entries and linked annotations/corrections as specified above. At the shared Log writer `src/queries.rs::append_prepared_log`, reject a fresh append whose id is already held by a legacy `objects` row, including `BatchWrite::Log`, within the admission transaction and independently of the origin Device or idempotency key; preserve ordinary successful Log replay. Inventory any legacy `objects` rows of this type and any ids shared by `objects` and `logs` before repair; preserve policy-permitted originals and links, and surface conflicting identities through ordinary review rather than overwriting or silently dropping history. Attach the running-revision reproduction and UI/CLI-to-writer trace when established.

The same remediation ticket must validate `log_annotation_added`/`annotation_of` and `log_correction_added`/`correction_of` at `src/queries.rs::append_prepared_log`, within the admission transaction and before a new entry or receipt is written. Require the applicable link to identify a distinct admitted original under current authority and visibility policy. Drafts and advisory candidates cannot establish a Log target; unresolved legacy identity collisions use ordinary review. Missing required imported history remains staged under existing dependency rules. Policy-permitted minimized target metadata may establish a link after authorized payload removal; validation must not retrieve or reconstruct purged content.

At the same shared `append_prepared_log` boundary, compare the complete immutable Log request with the admitted result before returning replay success: `id`, `event_type`, `object_refs`, `payload`, `provenance` and `created_at`, including annotation/correction links wherever represented. Use canonical JSON comparison for JSON fields. Reuse of the Device-global key with different immutable content must return `idempotency_key_conflict`; exact permitted replay returns the original result without writes. Preserve other writers' replay behavior. Existing payload-only receipts require comparison with their policy-permitted admitted Log record, without rewriting history or receipts. If retained metadata cannot establish equality after authorized redaction/purge, withhold replay success and surface the limitation through ordinary review; neither reconstruct purged content nor retain a denied comparison copy.

Extend `tests/admit_object.rs`, `tests/admit_batch.rs` and `tests/append_log.rs` to reject generic creates and valid-version updates of synthetic legacy `LogEntry` records, reject a batch containing the bypass without partial records or admission receipts, and prove originals survive linked correction and replay. Remove the cross-writer fixture's reliance on admitting `LogEntry` through ordinary-object admission while retaining Device-global idempotency coverage. These checks supplement the full closure evidence below; no implementation checks have been run by this documentation audit.

Extend `tests/append_log.rs` and `tests/admit_batch.rs` with a synthetic legacy `objects` row and a Log append claiming its id under a fresh key, with no target-absence precondition; exercise both the same origin Device and a different origin Device. Rejection must preserve the original row and existing receipts, create no Log or mutation receipt, and roll back any earlier valid writes in the batch. Keep fresh-id Log append, linked correction and successful replay coverage passing. These are required checks, not passing results.

Extend `tests/advisory_candidates.rs` with otherwise valid reviewable candidates submitting a generic `LogEntry` create and a valid-version update of a synthetic legacy `LogEntry`. Rejection must preserve the original record and the candidate's payload, version, lifecycle and admission links, with no new object/Log row, admitted candidate decision or mutation receipt. Existing decision events and receipts remain unchanged, and the candidate remains reviewable. Keep ordinary candidate-to-object admission coverage passing. These are required checks, not passing results.

Extend `tests/append_log.rs` and `tests/admit_batch.rs` with otherwise valid annotation and correction requests missing their applicable link, using a wrong-kind reference, referencing an unadmitted entry or pointing to the new entry itself. Rejection must preserve originals, prior links and existing receipts, create no Log or mutation receipt, and roll back earlier valid batch writes. Also prove valid target-then-correction admission within one batch, links to permitted minimized metadata after media deletion, and exact replay without duplicate entries. These are required checks, not passing results.

Extend `tests/append_log.rs` and `tests/admit_batch.rs` with otherwise valid retries preserving the Device/key and payload while changing each of `id`, `event_type`, `object_refs`, `provenance` and `created_at` independently. Each must conflict, preserve originals, prior links and existing receipts, create no new Log or receipt, and roll back earlier valid batch writes. Include linked annotation/correction fixtures, changed-link conflicts and exact replay controls, plus legacy payload-only receipts and unavailable comparison content after authorized purge. Exact permitted replay must return the original entry without duplicate corrections or writes; unavailable content cannot be reconstructed to decide equality. These are required checks, not passing results.

### Closure evidence

Close this question only with a revision-pinned reproduction, an implementation remediation ticket naming the affected paths, and passing behavioral checks showing:

- Annotation, correction and revocation leave the admitted original and prior links unchanged, create the appropriate linked entries, and expose both corrected meaning and permitted historical assertions. Snapshot replacement and required compensation remain explicit separate records.
- Application-owned direct and generic edit/delete bypasses fail; duplicate retry/replay adds no second entry or repeated compensation; stale, unauthorized and conflicting requests cannot rewrite history. Interrupted admission leaves no partial correction/compensation result.
- Query, import/replay and relevant planning inputs respect the admitted links; authorized media deletion leaves evidence unavailable with policy-safe metadata and no recoverable denied copy in dependent views.

### Resolution

Open. Append-only correction is already decided. Static source audits identify a generic `LogEntry` admission/update gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation and incomplete Log replay comparison at the shared Log writer. The proposed remediation includes shared-boundary guards, fresh-key legacy identity-collision checks, candidate rollback checks, linked-entry validation and complete immutable-record replay checks. The reported operation's revision-pinned reproduction, filed remediation ticket and passing behavioral evidence remain outstanding; this documentation patch changes no store or UI behavior.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0162-linked-history-discovery-20261015",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0162",
    "base_commit": "dce1b35c49756da059c6078d136bc93e446e95da",
    "summary": "Document the linked-history discovery gap and add read-side remediation checks to UBU-Q0162.",
    "rationale": "Valid correction admission alone cannot establish corrected query behavior: the inspected history reader searches only raw object references, and replay delegates to it. Corrections linked only to an original Log id can be omitted from domain-object history. Extend the existing implementation handoff under UBU-D0071 with source-pinned reader evidence, link-aware discovery and behavioral acceptance checks. Keep the question open pending the reported-operation reproduction, filed ticket and passing implementation evidence.",
    "changed_files": [
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3051,68 +3051,74 @@\n ## UBU-Q0162: Enforce append-only Log correction rather than in-place editing\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n ### Question\n \n Where does the operator-reported editable Log violate the existing append-only contract, and what minimum implementation correction closes it?\n \n ### Current direction\n \n-Append-only Logs and linked corrections are already design rules under `UBU-D0071`, not a newly adopted requirement. Audit edit paths and distinguish annotation, corrected query views and compensating entries from changing admitted history. Preserve original assertions, correction/revocation links and privacy-minimized media-deletion metadata. Local source audits identify a generic `LogEntry` admission gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation and incomplete Log replay comparison at the shared Log writer; the reported edit path, filed remediation ticket and passing implementation evidence remain outstanding. This documentation ticket changes no store or UI behavior.\n+Append-only Logs and linked corrections are already design rules under `UBU-D0071`, not a newly adopted requirement. Audit edit paths and distinguish annotation, corrected query views and compensating entries from changing admitted history. Preserve original assertions, correction/revocation links and privacy-minimized media-deletion metadata. Local source audits identify a generic `LogEntry` admission gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation, incomplete Log replay comparison at the shared Log writer and missing linked-history discovery; the reported edit path, filed remediation ticket and passing implementation evidence remain outstanding. This documentation ticket changes no store or UI behavior.\n \n ### Audit and minimum repair\n \n Record a privacy-safe reproduction identifying the running repository/revision, reported control or command, affected entry reference, and admitted record before and after the operation. Trace that operation through its UI/CLI, handler, admission and store writer; inventory other application-owned update, delete, replacement, import and replay paths that can reach the same records. Attach actual file/symbol/route references to the remediation ticket. An editable draft, changed corrected view, rebuilt index or reserialized storage file alone is not evidence that admitted history changed.\n \n The minimum repair uses the existing Log envelope and event types:\n \n - Route added notes through `log_annotation_added` with `annotation_of`; route withdrawal or replacement of an assertion through `log_correction_added` with `correction_of`. Each is a separately admitted entry with a fresh `log_entry_id`, correcting actor/Device, provenance, admission time and idempotency key. Annotation or semantic correction keeps the original envelope, assertion and existing links unchanged. Generic edit/delete paths must reject attempts to change admitted history, including its notes and provenance; an operator's edit authority does not waive this invariant.\n - Resolve current query views through admitted correction/revocation links while retaining policy-permitted historical views. A replacement Snapshot creates a new Snapshot and links it under DESIGN.md \u00a712.1. Correcting a Log assertion does not automatically undo Task effects, UniverseState mutations or external actions; any compensating transition needs its own ordinary admission and authority.\n - Reuse current authority, policy, observed-version and idempotency checks (`UBU-D0258`). Commit the new entry, admission outcome and any required canonical compensation atomically; invalidate affected derived state through ordinary recalculation. Stale or competing corrections use the ordinary conflict rules (`UBU-D0248`), not an unconditional overwrite or timestamp-only choice.\n - Keep policy-authorized media deletion, redaction and purge distinct from semantic correction. Preserve only permitted minimized correction/revocation/deletion links and outcome metadata under DESIGN.md \u00a712.2 and the existing Compartment rules. Neither originals nor correction payloads, indexes or derived views may retain or reconstruct content whose policy requires removal. Deletion must not fabricate replacement evidence or imply that an unconfirmed remote purge completed.\n \n ### Source audit and remediation handoff\n \n A 2026-10-12 static source audit of the clean local `ubu-store` checkout at `83a099f55220b1286ffb2dee3c9d19006e2090b5` identifies an alternate mutable representation of `LogEntry`:\n \n - `src/admission.rs::object_type_from_str` accepts `LogEntry`, and `validate_object_record` permits it as a `NewObjectRecord`. `src/queries.rs::admit_object` reaches `admit_prepared_object` and `write_object_row`, whose `VersionRef::Version` branch updates `objects` without a `LogEntry` guard. `admit_batch` with `BatchWrite::Object` reaches the same writer.\n - The source of `tests/append_log.rs::cross_writer_key_is_global_per_device` deliberately admits a `LogEntry` into `objects`. The dedicated `src/queries.rs::append_log_entry` and `append_prepared_log` instead insert into `logs`. This source audit does not establish which path the operator's reported control reaches or demonstrate a changed admitted record at runtime.\n \n A 2026-10-13 follow-up static audit of the same clean `ubu-store` revision also identifies `src/candidates.rs::admit_advisory_candidate`: it accepts a `NewObjectRecord` and calls `src/queries.rs::admit_prepared_object`, reaching `write_object_row` before `src/candidates.rs::write_decision` records candidate acceptance. There is no `LogEntry`-specific guard on this route. Guarding only the public `admit_object` wrapper or batch dispatch would leave candidate acceptance able to reach the generic create/update gap. This is another source-level reachability finding, not a reproduction of the reported control.\n \n A 2026-10-14 static audit of the same clean `ubu-store` revision identifies a related legacy identity-collision path: `migrations/0001_initial.sql` declares separate primary keys for `objects.id` and `logs.id`; `src/queries.rs::append_prepared_log` inserts into `logs` without checking for an existing `objects` row with that id. Log append requires no absence precondition for its own id. The replay-target check in `prepare_mutation` and `tests/append_log.rs::cross_writer_key_is_global_per_device` cover reused keys, leaving a fresh key able to claim a legacy `LogEntry` id in `logs`. This is a source-level finding about identity reuse, not a runtime reproduction of the reported edit.\n \n A 2026-10-14 static audit of the same clean `ubu-store` revision identifies missing correction-link validation: `src/models/log_record.rs::NewLogRecord` accepts a string `event_type` and arbitrary JSON `payload`; `src/queries.rs::append_prepared_log` validates the Log id, provenance and timestamp, but applies event-specific validation only to `recalculation_requested`. It does not validate `annotation_of` or `correction_of` before inserting into `logs`. Direct Log append and `BatchWrite::Log` share this path. Blocking generic `LogEntry` writes therefore leaves linked-correction validation outstanding. This is a source-level finding, not a reproduction of the reported edit or a passing behavioral result.\n \n A 2026-10-15 static audit of the same clean `ubu-store` revision identifies incomplete Log replay comparison: `src/queries.rs::append_log_entry` and the `BatchWrite::Log` branch of `admit_batch` pass only `record.payload` to `prepare_mutation`. With that payload unchanged, `append_prepared_log` returns the recorded Log result before comparing the requested `id`, `event_type`, `object_refs`, `provenance` or `created_at`. A changed request can therefore be reported as a successful replay of a different immutable record. This does not rewrite the original; it can falsely acknowledge a requested annotation or correction. The inspected `tests/append_log.rs::log_empty_preconditions_admit_and_replay_without_writes` and `log_changed_payload_conflicts_without_changes` cover exact replay and changed payloads, not these unchanged-payload changes. This is static evidence, not a runtime reproduction or passing check.\n \n+A 2026-10-15 read-side static audit of the same clean `ubu-store` revision identifies missing linked-history discovery: `src/queries.rs::get_object_history` searches only `object_refs_json` with SQL `LIKE` and orders by `created_at`; `src/replay.rs::replay_state` delegates directly to it. A correction referencing only the original Log entry, with `correction_of` in its payload, is omitted when querying the original domain object unless its id is repeated in the correction's `object_refs`. Neither function traverses correction/annotation links or resolves corrected meaning. `tests/replay_state.rs::replays_object_history_from_logs` appends one `object_admitted` entry and checks the history length; it does not cover linked corrections. Raw historical enumeration remains legitimate, but these functions alone do not establish corrected query semantics. This is static source evidence, not proof of downstream misuse or a reproduction of the reported edit.\n+\n Proposed implementation remediation ticket: **Enforce Log-only admission for `LogEntry` and append linked corrections** in `ubu-store`; file it and link it to `UBU-Q0162`. Reject generic `LogEntry` creates and updates at the shared admission boundary, including `BatchWrite::Object` and `src/candidates.rs::admit_advisory_candidate`, while retaining `LogEntry` in the ID registry. Use Log admission for fresh entries and linked annotations/corrections as specified above. At the shared Log writer `src/queries.rs::append_prepared_log`, reject a fresh append whose id is already held by a legacy `objects` row, including `BatchWrite::Log`, within the admission transaction and independently of the origin Device or idempotency key; preserve ordinary successful Log replay. Inventory any legacy `objects` rows of this type and any ids shared by `objects` and `logs` before repair; preserve policy-permitted originals and links, and surface conflicting identities through ordinary review rather than overwriting or silently dropping history. Attach the running-revision reproduction and UI/CLI-to-writer trace when established.\n \n The same remediation ticket must validate `log_annotation_added`/`annotation_of` and `log_correction_added`/`correction_of` at `src/queries.rs::append_prepared_log`, within the admission transaction and before a new entry or receipt is written. Require the applicable link to identify a distinct admitted original under current authority and visibility policy. Drafts and advisory candidates cannot establish a Log target; unresolved legacy identity collisions use ordinary review. Missing required imported history remains staged under existing dependency rules. Policy-permitted minimized target metadata may establish a link after authorized payload removal; validation must not retrieve or reconstruct purged content.\n \n At the same shared `append_prepared_log` boundary, compare the complete immutable Log request with the admitted result before returning replay success: `id`, `event_type`, `object_refs`, `payload`, `provenance` and `created_at`, including annotation/correction links wherever represented. Use canonical JSON comparison for JSON fields. Reuse of the Device-global key with different immutable content must return `idempotency_key_conflict`; exact permitted replay returns the original result without writes. Preserve other writers' replay behavior. Existing payload-only receipts require comparison with their policy-permitted admitted Log record, without rewriting history or receipts. If retained metadata cannot establish equality after authorized redaction/purge, withhold replay success and surface the limitation through ordinary review; neither reconstruct purged content nor retain a denied comparison copy.\n \n+Include the read side in the same remediation ticket at `src/queries.rs::get_object_history` and `src/replay.rs::replay_state`: discover history through exact structured object references and admitted annotation/correction links, including chains whose later entries reference only earlier Log ids. Preserve policy-permitted historical assertions and expose corrected interpretation separately under `UBU-D0071`; an annotation adds context without withdrawing the assertion. Resolve replacement/revocation through admitted links and ordinary conflict rules, never `created_at` ordering alone. Identify any callers using raw history as current truth and route them through the corrected view before claiming planning/query conformance. Apply current visibility, retention and dependency rules to link traversal; missing required history leaves the view explicitly incomplete within permitted visibility, and purged content cannot be fetched or reconstructed.\n+\n Extend `tests/admit_object.rs`, `tests/admit_batch.rs` and `tests/append_log.rs` to reject generic creates and valid-version updates of synthetic legacy `LogEntry` records, reject a batch containing the bypass without partial records or admission receipts, and prove originals survive linked correction and replay. Remove the cross-writer fixture's reliance on admitting `LogEntry` through ordinary-object admission while retaining Device-global idempotency coverage. These checks supplement the full closure evidence below; no implementation checks have been run by this documentation audit.\n \n Extend `tests/append_log.rs` and `tests/admit_batch.rs` with a synthetic legacy `objects` row and a Log append claiming its id under a fresh key, with no target-absence precondition; exercise both the same origin Device and a different origin Device. Rejection must preserve the original row and existing receipts, create no Log or mutation receipt, and roll back any earlier valid writes in the batch. Keep fresh-id Log append, linked correction and successful replay coverage passing. These are required checks, not passing results.\n \n Extend `tests/advisory_candidates.rs` with otherwise valid reviewable candidates submitting a generic `LogEntry` create and a valid-version update of a synthetic legacy `LogEntry`. Rejection must preserve the original record and the candidate's payload, version, lifecycle and admission links, with no new object/Log row, admitted candidate decision or mutation receipt. Existing decision events and receipts remain unchanged, and the candidate remains reviewable. Keep ordinary candidate-to-object admission coverage passing. These are required checks, not passing results.\n \n Extend `tests/append_log.rs` and `tests/admit_batch.rs` with otherwise valid annotation and correction requests missing their applicable link, using a wrong-kind reference, referencing an unadmitted entry or pointing to the new entry itself. Rejection must preserve originals, prior links and existing receipts, create no Log or mutation receipt, and roll back earlier valid batch writes. Also prove valid target-then-correction admission within one batch, links to permitted minimized metadata after media deletion, and exact replay without duplicate entries. These are required checks, not passing results.\n \n Extend `tests/append_log.rs` and `tests/admit_batch.rs` with otherwise valid retries preserving the Device/key and payload while changing each of `id`, `event_type`, `object_refs`, `provenance` and `created_at` independently. Each must conflict, preserve originals, prior links and existing receipts, create no new Log or receipt, and roll back earlier valid batch writes. Include linked annotation/correction fixtures, changed-link conflicts and exact replay controls, plus legacy payload-only receipts and unavailable comparison content after authorized purge. Exact permitted replay must return the original entry without duplicate corrections or writes; unavailable content cannot be reconstructed to decide equality. These are required checks, not passing results.\n \n+Extend `tests/replay_state.rs` with an original referencing a domain object, annotation/correction entries referencing only the original Log id, and a later correction linked to a correction. Prove the permitted history includes those entries with originals and prior links unchanged; the corrected view preserves an annotated assertion, applies uncontested replacement/withdrawal and leaves competing corrections subject to ordinary conflict review rather than choosing the latest timestamp. Include exact-reference negative fixtures, equivalent direct/batch admission and retry results, missing required history and permitted minimized links after authorized payload purge. Reads and repeated view resolution must create no Log, mutation receipt or compensation and must not reconstruct denied content. These are required checks, not passing results.\n+\n ### Closure evidence\n \n Close this question only with a revision-pinned reproduction, an implementation remediation ticket naming the affected paths, and passing behavioral checks showing:\n \n - Annotation, correction and revocation leave the admitted original and prior links unchanged, create the appropriate linked entries, and expose both corrected meaning and permitted historical assertions. Snapshot replacement and required compensation remain explicit separate records.\n - Application-owned direct and generic edit/delete bypasses fail; duplicate retry/replay adds no second entry or repeated compensation; stale, unauthorized and conflicting requests cannot rewrite history. Interrupted admission leaves no partial correction/compensation result.\n - Query, import/replay and relevant planning inputs respect the admitted links; authorized media deletion leaves evidence unavailable with policy-safe metadata and no recoverable denied copy in dependent views.\n \n ### Resolution\n \n-Open. Append-only correction is already decided. Static source audits identify a generic `LogEntry` admission/update gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation and incomplete Log replay comparison at the shared Log writer. The proposed remediation includes shared-boundary guards, fresh-key legacy identity-collision checks, candidate rollback checks, linked-entry validation and complete immutable-record replay checks. The reported operation's revision-pinned reproduction, filed remediation ticket and passing behavioral evidence remain outstanding; this documentation patch changes no store or UI behavior.\n+Open. Append-only correction is already decided. Static source audits identify a generic `LogEntry` admission/update gap reachable through direct, batch and advisory-candidate admission, plus missing annotation/correction link validation, incomplete Log replay comparison at the shared Log writer and missing linked-history discovery. The proposed remediation includes shared-boundary guards, fresh-key legacy identity-collision checks, candidate rollback checks, linked-entry validation, complete immutable-record replay checks and link-aware history/corrected-view checks. The reported operation's revision-pinned reproduction, filed remediation ticket and passing behavioral evidence remain outstanding; this documentation patch changes no store or UI behavior.\n \n ---\n \n",
    "commit_message": "docs: audit linked Log history discovery for UBU-Q0162",
    "validation_notes": [
      "git apply --check passed against the current /home/sean/ubu-design working tree using the proposed patch on stdin; the index was not used as the baseline.",
      "Only the UBU-Q0162 block changes. Its single-line metadata, Open status and existing closure requirements are preserved; the diff includes its question heading and its own Resolution section.",
      "Static source audit inspected get_object_history, replay_state and tests/replay_state.rs in the clean /home/sean/ubu-phase2/ubu-store checkout at 83a099f55220b1286ffb2dee3c9d19006e2090b5.",
      "No implementation tests were executed and no repository files were modified. The reported edit reproduction, filed remediation ticket and passing behavioral evidence remain outstanding."
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
    "proposal_id": "openai-ubu-q0162-linked-history-discovery-20261015",
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
