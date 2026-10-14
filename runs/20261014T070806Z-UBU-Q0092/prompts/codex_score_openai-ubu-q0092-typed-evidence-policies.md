# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0092`  
Question title: `Typed Relationship evidence policies`  
Base commit: `2938bdec36cab6a41917580384c24fe88a7c5cbf`

```markdown
## UBU-Q0092: Typed Relationship evidence policies

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0028, UBU-Q0083 Blocks: extrospection evidence, Relationship evidence retention, compartment-aware review Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

What typed retention, redaction, and evidence-use policy enums are required for Relationship evidence?

### Subquestions

1. Which retention policies are needed: retain indefinitely, retain until Relationship closes, session-only, auto-delete after duration, retain summary only, or user review required?
2. Which redaction policies are needed: full text permitted, summary only, selector only, reference only, metadata only, or prohibited?
3. Which EvidenceUsePolicy values govern extrospection, introspection, AssociationAttestation, planning, export, and model training?
4. How should raw excerpts be displayed without becoming durable copied private text?
5. How should EvidenceRefs record Compartment, source, timestamp range, selector, generated summary, permission basis, and provenance?

### Current direction

Evidence policies must be typed and enforceable. Durable Relationship evidence should prefer references and summaries over copied private excerpts.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0092-typed-evidence-policies",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0092",
    "base_commit": "2938bdec36cab6a41917580384c24fe88a7c5cbf",
    "summary": "Define typed Phase 2 Relationship evidence retention, redaction and use policies, source-bound EvidenceRefs and transient excerpt review; resolve UBU-Q0092 through UBU-D0352.",
    "rationale": "Separating lifetime, retained form and review avoids treating summary-only storage or approval as a deletion deadline. Purpose-specific permissions prevent evidence reuse from inheriting unrelated authority, while Compartment and ContextBundle rules continue to govern disclosure. Versioned provenance, finite previews and derivative cleanup make the contract auditable without extending Phase 1 privacy claims.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5516,3 +5516,17 @@ Generative presentation is optional advisory content or validated composition of\n Phase 1 retains its frozen one-next-Task explanation and inspection loop. Phase 2 extends supported review surfaces; richer communication, Association/governance and publication routes keep their existing release gates. This selects the UX contract, adds no canonical transition object or kernel field, and grants no new execution or capture authority. DESIGN.md \u00a721.9 specifies behavioral acceptance evidence; implementation conformance remains to be demonstrated.\n \n ---\n+\n+## UBU-D0352: Relationship evidence separates typed retention, redaction and permitted uses\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a722.2.1. Resolves `UBU-Q0092`; refines `UBU-D0074`/`UBU-D0162` without expanding Phase 1's implemented privacy promise.\n+\n+Phase 2 EvidenceRefs bind versioned policies with independent retention lifetime, retained form and retention-review requirements; explicit redaction representations; and purpose-specific `deny`, `allow` or `require_review` decisions. References and authorized summaries are preferred to copied private text. Missing policy fails closed; source/Compartment denials remain binding across reads, generation, derivatives, sync and export.\n+\n+Extrospection, introspection, AssociationAttestation, planning and export need their own scoped permission. Permission does not admit a finding or authorize an external action. Model training and provider training/secondary reuse remain denied for Phase 2 Relationship evidence. Raw review excerpts use bounded transient leases; copying, saving and disclosure require separate current checks.\n+\n+EvidenceRefs pin source/version, selectors/time ranges, Compartments, permission basis and provenance; generated summaries remain source-bound derivatives. Expiry blocks access before cleanup, and authorized purge removes reconstructive derivatives while retaining only permitted minimized audit/causal records. Unconfirmed offline cleanup is never reported as enforced or physically erased.\n+\n+DESIGN.md \u00a722.2.1 defines enum values, parameter requirements, policy intersections and behavioral conformance gates. This accepts a typed enforceable contract, not passing implementation evidence or a forensic-erasure guarantee; supported Phase 2 paths must demonstrate enforcement before enablement.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -4471,6 +4471,54 @@ Extrospection findings use an explicit lifecycle:\n \n Unreviewed findings must not silently update durable Relationship state. Rejected findings should remain durable enough to suppress repeated bad framings. Deferred findings may resurface when materially new evidence accumulates. High-frequency extrospection sessions without model updates may trigger a rumination advisory, but such behavioral-risk detection is advisory by default.\n \n+#### 22.2.1 Typed Relationship evidence policies\n+\n+**Phase 2 contract (`UBU-D0352`).** Each Relationship `EvidenceRef` binds versioned retention, redaction and evidence-use policies. These are hard authorization/storage boundaries under \u00a7\u00a72.11, 21.3 and 23, not bypassable behavioral safeguards. Phase 1 retains `UBU-D0074`'s narrower promise; recording a policy is not evidence of enforcement. Unsupported policies make the affected capture, retention or use unavailable.\n+\n+**Retention.** `EvidenceRetentionPolicy` separates `lifetime: EvidenceRetentionLifetime`, `retained_form: EvidenceRetainedForm` and `review: EvidenceRetentionReview`; summary-only storage and review requirements do not replace a deletion deadline.\n+\n+| `EvidenceRetentionLifetime` | Required binding and behavior |\n+|---|---|\n+| `indefinite` | Explicit authorized retention choice; no evidence-specific deadline, but source expiry, revocation and stricter policy still apply. |\n+| `until_relationship_closed` | Bound Relationship and an explicit authorized admitted closure marker/rule; expire on that marker, with a finite fallback `expires_at`. No inference from silence, refusal or Objective completion. Unsupported closure tracking makes this mode unavailable. Reopening cannot restore expired content. |\n+| `session_only` | Bound session and finite non-renewing `expires_at`; expire at session end or the deadline, whichever comes first. No durable evidence payload, including summaries. Restart cannot resume the lease. |\n+| `delete_after_duration` | Positive finite `duration_seconds`, fixed `collected_at` and derived `expires_at`; reading, copying, retrying or summarizing cannot restart the clock. |\n+\n+`EvidenceRetainedForm` is `reference_only`, `summary_only` or `selected_content`. `reference_only` stores only permitted refs, bounded non-content selectors and necessary provenance. `summary_only` additionally permits a derived summary, never copied excerpts. `selected_content` additionally permits purpose-required bounded source text only when redaction and source policy allow it. Each retained artifact has an explicit lifetime within its source-policy bounds; selecting summaries never grants them indefinite retention. Evidence-copy expiry does not itself delete an independently managed upstream archive or prove a historical claim false. Deadlines remain bound to originating capture/session, never another Device's receipt time; inability to establish an unexpired grant fails closed.\n+\n+`EvidenceRetentionReview` is `not_required` or `required`. `required` blocks durable capture until explicit user approval binds source/evidence versions, retained form, lifetime, scope and policy versions. Pending previews obey finite session-only limits. `not_required` needs an existing scoped user-approved retention rule. New policies default to `session_only`, `reference_only` and `required`; absent or unknown policies grant no access or reuse.\n+\n+**Redaction.** `EvidenceRedactionPolicy` is a maximum permitted representation for a named purpose, actor and audience/destination, not a claim that an artifact is harmless.\n+\n+| Value | Permitted representation |\n+|---|---|\n+| `full_text_permitted` | Minimum task-required selected source text and permitted supporting fields; no automatic full-archive access or durable copy. |\n+| `summary_only` | Permitted summary and necessary provenance refs/metadata; no excerpt or content-bearing selector. |\n+| `selector_only` | Permitted source ref and bounded non-content selector; no source text or summary. |\n+| `reference_only` | Authorized opaque reference with necessary minimal provenance; no selector, summary or source text. |\n+| `metadata_only` | Allowlisted non-reconstructive metadata; no source handle, selector, summary or source text. |\n+| `prohibited` | No evidence disclosure to that consumer, including refs, metadata or existence. |\n+\n+Modes such as summary-only and selector-only are not interchangeable levels. Intersect the actual permitted fields and operations across all contributing policies; if no useful safe representation exists, exclude or deny. Selectors, hashes, source ids, times and Compartment labels are protected data, not inherently safe metadata. A selector containing quoted text counts as source payload. Approved redaction transforms carry provenance; ordinary summarization cannot relax inherited restrictions or turn a private summary into an exportable structural reference.\n+\n+**Use.** `EvidenceUsePurpose` is `review`, `extrospection`, `introspection`, `association_attestation`, `planning`, `export` or `model_training`. `EvidenceUsePolicy` maps each purpose to `deny`, `allow` or `require_review`, with Relationship/Identity/Association scope, allowed audience/destination, expiry and permission-basis refs. Missing/unknown purposes or decisions deny. New policies deny reuse until an authorized scoped rule is supplied. `allow` permits only the named use within current source authority; `require_review` additionally requires operation-bound approval; `deny` cannot be bypassed by review. Revising a policy requires authority over its source restrictions.\n+\n+Extrospection permission cannot authorize an introspection handoff, another Relationship, AssociationAttestation, planning or export. Handoffs recheck both purposes and scope; cross-Relationship inference remains denied by default. Attestation and planning permissions do not admit findings, establish counterparty intent or create planner constraints: ordinary candidate review, authority and admission still apply. `export` covers clipboard, download, projection and disclosure outside the authorized Identity/Association or execution boundary. Such disclosure additionally requires export permission; replica sync separately checks replication policy and destination-specific evidence scope. Existing Compartment routing/replication and ContextBundle approval requirements apply even when use is `allow`. In Phase 2, `model_training` must be `deny`, including fine-tuning and provider training/secondary reuse; an inference approval cannot enable it. Destinations whose effective training/secondary-use behavior cannot satisfy this restriction are unavailable for Relationship evidence.\n+\n+**EvidenceRef and derivation.** Minimum fields are `evidence_ref_id`, `schema_version`, `relationship_refs` and perspective/actor Identity refs; source kind/system and policy-protected `source_ref`; pinned source version/snapshot and optional policy-permitted digest; bounded typed selector and half-open source timestamp range with timezone/clock basis or explicit unknown/not-applicable values; `collected_at` and `recorded_at`; `compartment_ids` (empty means explicitly low-security, not protected); versioned retention/redaction/use-policy refs; permission-basis authority/grant/consent refs and their scope/expiry; and actor/Device, admission/Log/SyncStatement and originating ContextBundle provenance when applicable. Broad archive names alone are insufficient selectors. Required unavailable source/version bindings block use rather than resolving to the latest text.\n+\n+An optional `generated_summary_ref` identifies a separately policy-bound derivative with source ids/versions/selectors, generation time, transform/model/template versions, coverage gaps, uncertainty and review state. Link corrections/supersession and all contributing parents; summaries and duplicate refs to the same interaction are not independent evidence or corroboration. Purpose-specific transition/Objective links preserve \u00a722.3.1's attribution. Source-policy changes constrain existing derivatives immediately; a saved permission basis is provenance, not a perpetual grant.\n+\n+**Transient excerpt review.** Fetch a bounded excerpt from the currently authorized pinned source through a purpose/actor/Device-bound finite view lease. Display it as temporary source text with permitted provenance and an expiry indicator. A raw preview requires source/read authority and `review` permission, and does not imply retention, downstream-use or export permission. Do not embed preview text in durable findings, Task descriptions, Logs, saved prompts, search indexes, telemetry, autosave, backups or default clipboard content. A copy/save/export request is a separately checked operation under the corresponding policies.\n+\n+Inventory and bound view/runtime buffers, scratch and caches, including applicable assistive buffers. End/lock, expiry, revocation, cancellation and crash/restart invalidate leases and clean disallowed app-controlled copies before reuse; reads never renew deadlines. If the deployed path cannot meet effective retention requirements, disable that raw-preview path and offer a permitted reference/summary. This controls UbU copies, not arbitrary screenshots, a compromised OS or forensic erasure.\n+\n+**Enforcement and audit.** Recheck current authority, source and policy versions, expiry, purpose, scope, Device eligibility and destination before read/assembly, before invocation or disclosure, and before derivative admission, sync or export. Approval binds those versions, selected representation and actual destination/profile; material changes invalidate it. Multi-source artifacts inherit every parent's restrictions and earliest applicable deadline unless a separately authorized proven redaction transform establishes a permitted artifact policy under the existing Compartment contract. Missing, conflicted or unavailable policy/authority fails closed. Local inference and user approval cannot override hard denials.\n+\n+Expiry/revocation first blocks reads, release and reuse even if physical cleanup is pending. Authorized redaction/purge removes disallowed payload and reconstructive derivatives, including summaries, embeddings, indexes, cached context and dependent prose, using ordinary correction/redaction statements. An immutable used ContextBundle does not mandate retaining its raw payload. Keep only policy-permitted minimized decision, gap, causal and idempotency records; audit must not become a second private-text archive or reveal denied existence. Track cleanup outcomes per Device; offline/unreachable copies remain unconfirmed under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a718. Neither logical deletion nor an unconfirmed cleanup proves physical erasure.\n+\n+Before claiming enforcement, require paired allowed/denied fixtures for every lifetime/form/review mode and use purpose; summary/selector/reference/metadata boundaries and denied existence; changed-source/policy approval races; mixed Compartments and hard route denials; rejected introspection/Association/planning handoffs and training destinations; excerpt expiry, lock, cancellation and crash/restart without durable preview copies; derivative/index/cache purge, replay without lease renewal, and unconfirmed offline cleanup. Verify policy-safe Logs and unchanged candidate/admission boundaries. These are required implementation gates, not reported passing tests; this decision resolves the typed Phase 2 contract only.\n+\n ### 22.3 RelationshipScopeTransition\n \n Users may deliberately create Objectives to change their own participation in a Relationship, invite or test mutual scope changes, or clarify whether another party is willing to enter a different scope. Examples include exploring whether a friend is open to a romantic scope, downscoping a close friendship to a formal-acquaintance scope, formalizing an observed professional collaboration, or clarifying an ambiguous drift.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1292,27 +1292,27 @@ Open.\n \n ## UBU-Q0092: Typed Relationship evidence policies\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0028, UBU-Q0083 Blocks: extrospection evidence, Relationship evidence retention, compartment-aware review Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0028, UBU-Q0083 Blocks: extrospection evidence, Relationship evidence retention, compartment-aware review Resolved by: UBU-D0352 Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n What typed retention, redaction, and evidence-use policy enums are required for Relationship evidence?\n \n ### Subquestions\n \n 1. Which retention policies are needed: retain indefinitely, retain until Relationship closes, session-only, auto-delete after duration, retain summary only, or user review required?\n 2. Which redaction policies are needed: full text permitted, summary only, selector only, reference only, metadata only, or prohibited?\n 3. Which EvidenceUsePolicy values govern extrospection, introspection, AssociationAttestation, planning, export, and model training?\n 4. How should raw excerpts be displayed without becoming durable copied private text?\n 5. How should EvidenceRefs record Compartment, source, timestamp range, selector, generated summary, permission basis, and provenance?\n \n ### Current direction\n \n Evidence policies must be typed and enforceable. Durable Relationship evidence should prefer references and summaries over copied private excerpts.\n \n ### Resolution\n \n-Open.\n+Resolved by UBU-D0352. DESIGN.md \u00a722.2.1 defines typed retention lifetime/form/review controls, redaction representations, purpose-specific EvidenceUsePolicy decisions, transient excerpt handling and source-bound EvidenceRef provenance. Hard source/Compartment denials and ContextBundle approval remain binding; Phase 2 training/secondary reuse is denied. This resolves the design contract; implementation enforcement and cleanup require the specified conformance evidence and do not expand Phase 1 privacy claims.\n \n ---\n \n",
    "commit_message": "docs: define typed Relationship evidence policies (UBU-Q0092)",
    "validation_notes": [
      "Generated hunks with git diff --no-index from unchanged current working-tree copies and proposed temporary copies; normalized only file headers. Temporary copies were removed and repository files were not modified.",
      "git apply --check and git apply --check --whitespace=error passed against the current working tree; no cached/index-only validation was used.",
      "Verified JSON schema fields, accurate hunk counts, unique question/decision headings, unused UBU-D0352, one question hunk containing UBU-Q0092 and its own Resolution, unchanged neighboring blocks and preservation of every other metadata field.",
      "Required Phase 2 conformance fixtures are specified, not executed; this documentation proposal claims no implemented enforcement, confirmed offline purge or forensic erasure.",
      "Human approval is required for this security policy, including the proposed Phase 2 model-training/provider-secondary-use denial."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0092"
    ],
    "decisions_added": [
      "UBU-D0352"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-ubu-q0092-typed-evidence-policies",
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
