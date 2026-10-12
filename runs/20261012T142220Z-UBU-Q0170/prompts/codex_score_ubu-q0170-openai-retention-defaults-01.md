# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0170`  
Question title: `Default raw-media retention for consented perception`  
Base commit: `9d4645b3976c1a2c647d4ab57c91b318476546fb`

```markdown
## UBU-Q0170: Default raw-media retention for consented perception

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None

### Question

What default retention applies to raw media in an explicitly enabled continuous or near-continuous perception session?

### Current direction

Delta §17.8. Minimize retained raw surveillance under DESIGN.md §12.2. Decide transient buffers, purposeful clips, local storage eligibility and expiry, without confusing deletion of media with editing append-only historical evidence. Do not promise automated retention enforcement before implementation.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0170-openai-retention-defaults-01",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0170",
    "base_commit": "9d4645b3976c1a2c647d4ab57c91b318476546fb",
    "summary": "Default consented continuous perception to memory-only raw media for at most 60 seconds; require explicit approval for encrypted local clips with a 24-hour default expiry.",
    "rationale": "A 60-second ceiling permits brief look-back, and a 24-hour clip default permits same-day review without an implicit session archive. Acquisition-based deadlines prevent review queues, retries and copies from renewing retention. Purpose-scoped approval, current Compartment policy, truthful cleanup status and append-only history remain binding. These are proposed privacy ceilings requiring human approval and implementation evidence, not measured performance claims.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2075,7 +2075,17 @@ Allowed Phase 1 discovery inputs are intentionally narrow:\n \n Phase 1 discovery excludes covert or broad capture by default: continuous microphone, camera, screen recording, keystroke logging, raw message bodies, raw file contents, raw GPS trails, and cross-Identity sharing are outside the default discovery input set. Any later use of those sources requires a separate explicit mode, Compartment review, routing disclosure, and user approval.\n \n-**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Segmentation and bounded local interfaces follow \u00a721.1.5 (`UBU-D0326`); raw-media retention defaults and operator discard/export controls remain open in `UBU-Q0170`\u2013`UBU-Q0171`; recording a policy does not claim its enforcement has shipped.\n+**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Segmentation and bounded local interfaces follow \u00a721.1.5 (`UBU-D0326`); raw-media retention defaults follow `UBU-D0327` below, while operator discard/export controls remain open in `UBU-Q0171`; recording a policy does not claim its enforcement has shipped.\n+\n+**Default raw-media retention (`UBU-D0327`).** For an explicitly enabled continuous or near-continuous perception session, durable raw-media retention is off. Use a local memory-only rolling buffer with a maximum sample age of **60 seconds from acquisition**, measured on the original source monotonic clock. Release material earlier when its authorized processing/look-back purpose no longer needs it or a shorter policy limit applies. The finite byte and aggregate-resource limits required by \u00a721.1.5 may shorten this window; it is a privacy ceiling, not a promised look-back duration or latency result. Pins, worker copies, queues, retries and decoded frames share the original deadline. No raw disk spool, debug payload, general cache, automatic backup or sync copy is authorized by this default. A Device/backend profile whose persistence paths cannot meet effective policy is ineligible.\n+\n+On pause, end, source/Identity/Compartment change or revocation, invalidate affected reads and pending results immediately and release unsaved raw buffers and copies within \u00a721.1.5's bounded cancellation grace, without exceeding the original age ceiling. Resume starts a fresh source epoch. Pending review, an unfinished model job and session metadata do not keep raw media alive; expired spans become explicitly unavailable.\n+\n+A purposeful clip requires explicit operator approval before durable saving, bound to the selected source intervals, purpose, eligible capture Device, Compartment and current policy versions. A relevance suggestion or capture consent alone cannot approve it. Store only the bounded selected material in access-controlled encrypted local storage on that Device, on the operator's own hardware, with lineage and an absolute expiry. The default expiry is **24 hours after the earliest captured sample**, or an earlier source/Compartment deadline. Missing reliable capture-time mapping denies durable saving. Review, copying, reprocessing and restart never reset expiry. A longer finite lifetime requires a separate purpose-specific approval bound to the clip and current policies before expiry; expired material cannot be recovered by renewed approval. Durable saving remains unavailable if eligible storage, finite clip/quota limits or expiry enforcement are absent. Recheck current policy and Device eligibility on every access; capture-time approval is not continuing authority. Backup, another Device and export require their separately allowed route and approval; hard denials remain binding.\n+\n+Expiry denies further reads and result release and cancels affected jobs. Remove expired app-controlled clip payloads, any clip-specific keys and reconstructive raw copies; if cleanup is interrupted, reconcile deadlines and complete pending cleanup before reopening capture or retained-media access after restart. Clock rollback or uncertain remaining lifetime must deny access rather than extend retention. Report scheduled, pending, failed and confirmed local deletion distinctly; a deadline or local unlink cannot establish remote deletion or forensic erasure. App shutdown may leave cleanup pending and must not be reported as completed deletion.\n+\n+Do not automatically replace discarded raw media with a transcript, embedding, hash or summary archive. Separately retained minimized evidence needs its own allowed purpose and retention authority, inherits source disclosure restrictions, and may outlive raw media only when policy permits. Purge disallowed reconstructive derivatives and retain only permitted gap/deletion metadata. The append-only rules below continue to govern admitted claims. This is a policy decision, not implemented automated deletion or a physical-erasure guarantee; continuous capture and clip storage require the revision-pinned retention checks in `UBU-D0327` before enablement. Detailed operator controls remain `UBU-Q0171`.\n \n Selective raw-media retention and append-only evidence are distinct. Discarding a raw stream or deleting an authorized clip does not edit an admitted historical claim in place: append minimized correction, revocation, deletion or tombstone metadata with the appropriate links. The immutable Log retains evidence about what was asserted, reviewed or withdrawn, not an automatic reconstructible copy of private raw media. A hash is not proof of an event and can itself leak correlation; policy must govern it. Later views can mark deleted evidence unavailable without inventing a replacement observation. `UBU-Q0162` tracks the reported Log-editability enforcement gap.\n \n@@ -3348,7 +3358,7 @@ Evidence review/admission neither grants capture/export capability nor approves\n | New contradiction or operator correction after acceptance, changed use/versions/policy, and delayed model output | The old assessment/approval cannot authorize affected Plan reuse or execution; admitted corrections append history and trigger recalculation, and the late result cannot overwrite them. |\n | Sufficient evidence and admitted candidate, but no external-action approval or a revoked grant; apparent completion without effect evidence | No external dispatch or inferred completion/effect. Evidence sufficiency cannot replace action authorization or `UBU-Q0172`'s remaining proof requirements. |\n \n-`UBU-D0321` specifies qualifier content and fidelity gates; `UBU-D0324` specifies consequence-sensitive corroboration and review; `UBU-Q0166` retains revision-pinned host-mapping evidence and demonstrated schema-gap review. `UBU-D0326` specifies segmentation and scoped local analysis in \u00a721.1.5; `UBU-Q0170`\u2013`UBU-Q0171` retain raw-media retention defaults and operator controls; `UBU-Q0172` retains proof-of-effect semantics. This contract freezes bindings, reuse, corroboration and admission behavior without claiming the remaining questions or implementation acceptance are complete.\n+`UBU-D0321` specifies qualifier content and fidelity gates; `UBU-D0324` specifies consequence-sensitive corroboration and review; `UBU-Q0166` retains revision-pinned host-mapping evidence and demonstrated schema-gap review. `UBU-D0326` specifies segmentation and scoped local analysis in \u00a721.1.5; `UBU-D0327` specifies raw-media retention defaults in \u00a712.2; `UBU-Q0171` retains operator controls and `UBU-Q0172` retains proof-of-effect semantics. This contract freezes bindings, reuse, corroboration and admission behavior without claiming the remaining questions or implementation acceptance are complete.\n \n #### 21.1.2 Restricted-attention audio envelope\n \n@@ -3657,7 +3667,7 @@ Revocation/purge invalidates disallowed handles and removes app-controlled disal\n - Cancellation/revocation during read, inference and result delivery; delayed/duplicate callbacks; source changes, restart and handoff produce no stale release, revived consent or duplicate candidate effects, and release workers/artifacts within the configured bound.\n - Normal completion, expiry, crash/restart and policy purge remove disallowed scratch/caches/derivatives; separately retained evidence remains correctly policy-bound and ordinary review is required before canonical effects.\n \n-These are required gates, not reported passing tests or physical-erasure claims. `UBU-Q0169`'s architecture is answered; numeric latency/resource acceptance remains `UBU-Q0168`, default raw-media retention remains `UBU-Q0170`, and operator discard/summary/hash/clip/export controls remain `UBU-Q0171`. Continuous capture cannot be enabled with unresolved effective retention policy. Host fidelity, proof-of-effect semantics and delivery sequence retain their existing gates.\n+These are required gates, not reported passing tests or physical-erasure claims. `UBU-Q0169`'s architecture is answered; numeric latency/resource acceptance remains `UBU-Q0168`, default raw-media retention follows \u00a712.2 (`UBU-D0327`), and operator discard/summary/hash/clip/export controls remain `UBU-Q0171`. Continuous capture cannot be enabled with unresolved effective retention policy. Host fidelity, proof-of-effect semantics and delivery sequence retain their existing gates.\n \n ### 21.2 Structured-output admission pipeline\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5168,3 +5168,21 @@ Expose only `media.frames.extract`, `media.events.detect` and `media.sections.is\n Serialize cancellation/revocation with access and result acceptance, invalidate pending generations immediately and bound worker/resource release. Keep intermediates under finite leases, clean disallowed scratch/caches/derivatives on expiry, cancellation, purge and restart, and require separate allowed evidence-retention or export authority. Results remain Discovery/advisory evidence subject to existing fidelity, corroboration, review and admission rules; admitted history retains ordinary correction/redaction semantics.\n \n Require \u00a721.1.5's pinned segmentation, access-denial, resource, interruption and artifact-lifecycle fixtures before enablement. This resolves an architectural contract, not implementation, host compatibility, measured performance or physical deletion. `UBU-Q0168` retains numeric latency/resource acceptance, `UBU-Q0170` raw-media retention defaults, `UBU-Q0171` operator controls, `UBU-Q0166` host fidelity, `UBU-Q0172` proof-of-effect semantics and `UBU-Q0183` delivery sequence.\n+\n+---\n+\n+## UBU-D0327: Consented perception defaults to transient raw media and expiring local clips\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a712.2. Resolves `UBU-Q0170`'s retention-policy contract; complements `UBU-D0292` and `UBU-D0326` without enabling capture or claiming implemented deletion.\n+\n+Adopt zero durable raw retention by default, a memory-only buffer capped at 60 seconds from acquisition and earlier purpose/policy expiry, and bounded cleanup on session interruption. Pins, copies and model work cannot extend the deadline. Purposeful clips require explicit, version-bound operator approval and eligible encrypted storage on the capture Device; their default absolute expiry is 24 hours after the earliest captured sample. Longer finite retention needs separate purpose-specific approval before expiry. No backup, sync or export permission follows from capture or retention approval.\n+\n+Expiry blocks access; interrupted cleanup remains visibly pending until completed. Separately authorized minimized evidence may outlive raw media under its own permitted policy. Raw deletion never edits an admitted historical claim or creates a reconstructible substitute archive; permitted correction/deletion links preserve the distinction.\n+\n+Before enabling continuous capture or durable clip saving, pin implementation, Device/OS/runtime/backend and policy/profile revisions and demonstrate:\n+\n+- Sustained capture, slow/pinned consumers, pause/end, revocation and delayed results obey the 60-second or shorter deadline and aggregate byte limits without disk spill, hidden caches, debug copies, backup/sync or cloud fallback. Inventory OS/runtime swap, hibernation and crash-dump paths; leave profiles unavailable where effective policy cannot be enforced.\n+- Clip approval, storage eligibility and quotas fail closed; expiry, copy/retry, clock rollback, app shutdown, crash/restart and interrupted/failed cleanup cannot renew access or falsely report deletion. A shorter Compartment policy and a separately approved longer finite lifetime both retain their exact bindings.\n+- Purge removes disallowed app-controlled copies and reconstructive derivatives while admitted claims retain permitted append-only links and unavailable-source gaps. Summaries/hashes do not bypass source policy, and local cleanup never claims unverified remote or physical erasure.\n+\n+These are required behavioral gates, not passing results. `UBU-Q0171` retains detailed operator controls, `UBU-Q0168` measured latency/resource acceptance, `UBU-Q0177` hardware eligibility and `UBU-Q0162` the Log-editability enforcement gap. This decision changes no Phase 1 discovery default, wire schema, kernel API or existing release sequence.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3227,19 +3227,19 @@ Resolved by `UBU-D0326`: DESIGN.md \u00a721.1.5 defines the `local-media/1` broker a\n \n ## UBU-Q0170: Default raw-media retention for consented perception\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0327 Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n What default retention applies to raw media in an explicitly enabled continuous or near-continuous perception session?\n \n ### Current direction\n \n-Delta \u00a717.8. Minimize retained raw surveillance under DESIGN.md \u00a712.2. Decide transient buffers, purposeful clips, local storage eligibility and expiry, without confusing deletion of media with editing append-only historical evidence. Do not promise automated retention enforcement before implementation.\n+Delta \u00a717.8. `UBU-D0327` specifies zero durable raw retention by default, a memory-only buffer capped at 60 seconds from acquisition, and explicitly approved purpose-scoped clips on eligible encrypted local storage with a 24-hour default expiry from the earliest captured sample. DESIGN.md \u00a712.2 separates access expiry, pending/completed cleanup and append-only historical evidence. Automated enforcement remains an implementation gate, not a shipped claim.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0327`: DESIGN.md \u00a712.2 defines transient raw-buffer age and resource limits, interruption cleanup, explicit clip-retention approval, local storage eligibility and absolute expiry. Review, retries, copies and restart cannot renew deadlines; longer finite clip retention requires separate approval before expiry. Raw deletion preserves only permitted append-only claim/correction/deletion metadata, and minimized derivatives need their own policy authority. Revision-pinned retention and failure checks are required before enablement; this resolves the policy contract, not implementation or physical erasure. `UBU-Q0171`, `UBU-Q0168`, `UBU-Q0177` and `UBU-Q0162` retain operator-control, measured-performance, hardware and Log-enforcement scope.\n \n ---\n \n",
    "commit_message": "docs: define consented raw-media retention defaults (UBU-Q0170)",
    "validation_notes": [
      "Generated the patch with git diff --no-index against the current working-tree files and in-memory proposed contents; normalized file headers only.",
      "git apply --check --whitespace=error - passed against the current working tree; no --cached or --recount was used.",
      "Verified the OPEN_QUESTIONS.md hunk includes the UBU-Q0170 heading and its own Resolution section; only its status, resolving decision, direction and resolution change, with all other metadata preserved.",
      "Confirmed UBU-D0327 is the next unused decision id; git apply --numstat reports only the three allowed changed files.",
      "Verified original file hashes and git status are unchanged. No files were written; behavioral retention checks are proposed enablement gates, not executed implementation tests."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0170"
    ],
    "decisions_added": [
      "UBU-D0327"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0170-openai-retention-defaults-01",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
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
