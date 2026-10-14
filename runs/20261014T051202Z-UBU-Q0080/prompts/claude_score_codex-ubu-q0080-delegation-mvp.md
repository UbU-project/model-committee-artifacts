# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0080`  
Question title: `Delegation Substrate MVP schema and self-reminder use`  
Base commit: `6a3aae470f2a51591f560d8a0548ef1211d38c49`

```markdown
## UBU-Q0080: Delegation Substrate MVP schema and self-reminder use

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: Task delegation, Automation Worker assignment, solo Task formalization, future Skill Barter marketplace Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

What is the minimum Delegation Substrate schema that prepares Tasks for delegation while also helping the user perform solo Tasks more clearly?

### Subquestions

1. Which fields belong directly on Task versus a separate `DelegationPacket`?
2. What executor types are needed: self, local agent, remote/cloud agent, tool, human Identity, Association, General Contractor, or external provider?
3. What authority, evidence, privacy, expected-output, review, deadline, and escalation fields are required?
4. How should a solo Task use the same structure as a self-reminder without implying actual handoff?
5. Which subset is required for Phase 1 GitHub dogfooding and Automation Worker assignments?

### Current direction

The MVP should implement Delegation Substrate primitives where they support dogfooding, self-reminder clarity, and worker assignment. The full Skill Barter marketplace is not Phase 1.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0080-delegation-mvp",
    "provider_id": "codex",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0080",
    "base_commit": "6a3aae470f2a51591f560d8a0548ef1211d38c49",
    "summary": "Define optional Task execution briefs and versioned DelegationPackets for solo clarity, explicit worker handoff, and Phase 1 GitHub dogfooding.",
    "rationale": "Separate reusable Task intent, immutable handoff bindings, and existing worker assignment state. Make self-reminders useful without dispatch; preserve hard privacy gates, candidate review, finite execution limits, and human repository authority while deferring multi-party coordination and marketplace semantics.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -4046,29 +4046,53 @@ The minimum dogfooding fixture is a local loopback MCP adapter over synthetic or\n \n A Delegation Substrate packet prepares a Task for execution by making the work explicit enough to hand off, automate, review, or perform oneself with less ambiguity.\n \n-Provisional fields:\n+#### 21.5.1 Task brief and packet ownership\n \n-- `delegation_packet_id`;\n-- `task_ref` or `objective_ref`;\n-- `purpose`;\n-- `executor_type` (`self`, `local_agent`, `remote_agent`, `tool`, `human_identity`, `association`, `general_contractor`, `external_provider`);\n-- `executor_ref` when known;\n-- `authority_source`;\n-- `authority_scope`;\n-- `expected_output`;\n-- `required_evidence`;\n-- `review_requirement`;\n-- `privacy_scope`;\n-- `compartment_refs`;\n-- `allowed_tools`;\n-- `timebox_or_deadline`;\n-- `failure_path`;\n-- `escalation_path`;\n-- `status`.\n+The MVP contract (`UBU-D0342`) keeps reusable intent on Task and a version-bound handoff in a separate `DelegationPacket`. Task remains the canonical owner of its Objective links, description, status, timing, dependencies, preconditions, effects, External References, and Compartment/privacy metadata.\n+\n+Task may carry an optional `execution_brief` with `purpose`, `intended_executor_type` (default `self`), `expected_output`, `completion_criteria`, `required_evidence`, `self_authority_note`, `review_requirement`, and `escalation_notes`. It may also link `delegation_packet_refs`. Purpose explains why the work matters; expected output names the artifact or outcome and an optional output schema ref; completion criteria describe what must be checked; required evidence names the evidence kinds needed, rather than claiming evidence already exists. An explicit empty evidence list means an authorized completion declaration recorded in Logs is sufficient only where the applicable operation policy permits it.\n+\n+These fields are optional for ordinary solo capture and do not change \u00a79.3's schedulability invariant. Missing fields remain visibly unknown. Before automated dispatch, purpose, expected output, completion criteria, an explicit evidence rule, and review requirement must be resolved. Description prose and brief fields supply no hidden planner constraints: any typed timing, dependency, precondition, or effect change needs ordinary Task admission.\n+\n+A packet binds one Task. An Objective-only or Container-level preparation is a preview until ordinary admission creates or selects an executable Task; multi-executor work uses ordinary child Tasks and Container lineage. Packets contain a frozen brief snapshot and versioned source refs, not a second editable Task description or Task lifecycle. Task edits never silently rewrite a dispatched packet. Sensitive details in briefs and packets use policy-gated source refs rather than ordinary WorkItem payload.\n+\n+#### 21.5.2 Minimum packet schema\n+\n+All rows below are required on an admitted packet. Lists are explicitly empty when no applicable sources, grants, tools, or routes exist; optional limits may be absent. Draft candidates may remain incomplete and cannot execute.\n+\n+| Fields | Meaning and requirement |\n+| --- | --- |\n+| `delegation_packet_id`, `schema_version`, `packet_version`, `task_ref`, `expected_task_version` | Stable packet identity and the admitted Task version being prepared. |\n+| `execution_brief_snapshot`, `source_refs` | Resolved brief plus versioned input/evidence refs or hashes. Protected source payload is accessed through policy-gated context, not embedded in ordinary Task structure. |\n+| `executor_type`, `executor_ref` | Execution class and concrete Identity, worker, tool, or provider endpoint ref; the ref may be null during preparation, but must resolve before dispatch. |\n+| `authority_source`, `authority_scope`, `capability_grant_refs`, `allowed_tools` | Who authorizes the work, permitted operations and object/integration scope, current grants, and tool allowlist. Worker dispatch requires active parent-issued grants covering its operations. A solo self-authority note is descriptive; it grants no tool or external-action authority. |\n+| `review_requirement` | `{ mode, reviewer_identity_ref?, policy_ref? }`, with mode `self_review`, `human_review`, or `policy_review`. Human review requires a reviewer ref; policy review requires a previously admitted operation-scoped policy ref. Worker output defaults to human review; self review is for solo work. The effective rule is also recorded in the brief snapshot and cannot weaken Task or operation review requirements. |\n+| `privacy_scope`, `compartment_refs`, `originating_context_bundle_refs`, `route_decision_refs` | Source classification, allowed disclosure/destination scope, retention-policy refs, and context/routing provenance; each field inherits applicable source restrictions. Lists may be empty for an un-compartmented local self-reminder; boundary-crossing work requires the applicable ContextBundle and policy/route decisions. Un-compartmented content is explicitly `security_level: low`. |\n+| `execution_limits` | `{ max_elapsed_seconds?, max_calls?, deliver_by?, cost_limit? }`; automated work requires positive finite elapsed-time and call limits. `deliver_by` is an optional UTC output deadline. This record does not replace Task timing constraints or the assignment's lease/heartbeat deadline. |\n+| `failure_path`, `escalation_path` | Stop/clarify/review behavior on invalid output, denial, timeout, or unmet criteria, and the responsible reviewing Identity. Default: stop and return the unresolved work to the assigning user for review. Retry uses \u00a725.1.2; escalation grants no contact, broader context, reassignment, or subdelegation authority. |\n+| `created_by_identity_ref`, `created_at`, `provenance`, `idempotency_key`, `status`, `supersedes_packet_ref` | Authorship, audit/version lineage, deduplication, and preparation state; supersedes ref is null for the first revision. |\n+\n+Packet `status` is preparation state only: `not_delegated` for an admitted self-reminder, `prepared` for an admitted non-self preparation, and `superseded` or `cancelled` when retired through ordinary admission. Preparing a packet does not offer or assign work. An invocation binds the packet id/version; used packet content is immutable. Changes require a new revision, fresh bindings and any renewed approval. Retirement is recorded in append-only history; retention, redaction, and purge follow source policy.\n+\n+Execution status, lease, delivery, and assignment outcome remain in \u00a725.1.2's assignment record, linked to the packet id/version. Cancelling a preparation does not silently cancel an active assignment; the parent must admit and log its assignment transition. Actual output/evidence and its admission or rejection remain linked candidates and Logs, not mutable claims in the brief.\n+\n+#### 21.5.3 Executor types and self-reminders\n+\n+Preserve `self`, `local_agent`, `remote_agent`, `tool`, `human_identity`, `association`, `general_contractor`, and `external_provider`. The type describes intended execution, not authority or trust. Agent types distinguish execution location; cloud inference used by a local agent still needs \u00a72.11 provider routing. An external provider references its descriptor/endpoint, while any assigned worker remains a separately identified actor under a parent-issued grant.\n+\n+Phase 1 implements self-reminders and explicitly configured agent/tool/provider routes only to the depth required by dogfooding. Worker execution uses the existing assignment contract regardless of executor type. Human-Identity and Association handoff, General Contractor coordination/subdelegation, discovery, bidding, settlement, and marketplace lifecycles remain future work; preparing those types cannot dispatch unsupported execution.\n+\n+For `self`, no separate packet is required: display the Task brief as \"For me\", with purpose, expected result, evidence/checklist, constraints, and escalation notes. An optional saved packet uses `not_delegated`, resolves to the user's Identity, and creates no offer, worker lease, notification, recurring reminder, or external disclosure. Starting and completing the Task use ordinary Task/Log admission. A user-selected switch to a worker requires a prepared packet, policy checks, disclosure/approval where required, and explicit parent assignment.\n+\n+#### 21.5.4 Dispatch, review, and Phase 1 dogfooding\n+\n+`delegation_packet.prepare` produces a candidate under \u00a721.4; it cannot dispatch work. Before context assembly and again before invocation, the parent validates packet/Task versions, current grants and their versions, allowed operations/tools, executor eligibility, ContextBundle bindings, Compartment policy, disclosure approval, limits, and review policy. Approval binds the packet revision, source versions, destination and relevant policy/grant versions; material change invalidates it. Capability grants, packet notes and user approval cannot override hard Compartment, Identity-isolation, or EvidenceUsePolicy denials. Credentials remain scoped secret references in the existing provider/integration stores, never packet payloads.\n+\n+Outputs carry packet id/version, assignment ref when applicable, originating ContextBundle refs, evidence/provenance, idempotency key, and expected target versions through \u00a721.2/\u00a725.1.3 admission. Receipt, delivery, or assignment completion alone does not complete a Task, admit modeled effects, or satisfy an Objective. Those changes need separately validated evidence and authorized canonical admission. Assignment failure/expiry leaves work unresolved under \u00a725.1.2; duplicate, stale, revoked, and denied submissions cannot silently change it. Handoff alone never automatically marks the Task moot.\n \n-Status values may include `not_delegated`, `prepared`, `proposed`, `offered`, `accepted`, `in_progress`, `delivered`, `reviewed`, `rejected`, `completed`, and `cancelled`.\n+The Phase 1 slice is the optional Task brief, versioned packets for assigned work, worker Identity/grant and assignment links, minimized context and policy decisions, candidate output review, and append-only audit Logs. GitHub dogfooding reuses External References for Issue/PR inputs and returns analysis, patch artifacts, reconciliation reports, or projection previews. Live writes still require \u00a727.3 approval; model-committee retains \u00a73.10's advisory boundary and cannot apply canonical patches or mutate GitHub.\n \n-When `executor_type` is `self`, the packet acts as a self-reminder and execution aid. It does not imply external handoff.\n+Before claiming this slice implemented, require fixtures covering a solo brief without handoff, explicit worker assignment and delivery pending review, duplicate/stale/revoked or expired submissions, Compartment-denied handoff, permitted disclosed cloud routing, unsupported executor rejection, timeout/escalation without authority expansion, and GitHub preview without live mutation. These are acceptance requirements, not passing runtime evidence.\n \n ### 21.6 General Contractor\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5368,3 +5368,15 @@ UbU still owns bounded validator-safe producer grammars, finite request/output a\n Document adapter requirements and runtime configuration instructions. Record observable sampling configuration or its digest with qualified profiles, mark unknown effective defaults explicitly and repeat affected qualification checks after configuration changes where applicable under `UBU-D0331`/`UBU-D0336`. Require request-construction and failure-path fixtures before claiming conformance, as specified in DESIGN.md \u00a71.\n \n This settles runtime-control ownership, not the cause of the already-bounded Vocabulary abort. The separate SuggestTags defect remains evidence for producer bounds, not a sampler diagnosis. No passing adapter fixtures, supported runtime configuration or implementation change is established by this decision.\n+\n+---\n+\n+## UBU-D0342: Delegation MVP separates Task briefs, versioned packets, and worker assignments\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.5. Resolves `UBU-Q0080`; refines `UBU-D0137` within `UBU-D0074`, `UBU-D0157`, and `UBU-D0177` boundaries.\n+\n+Keep reusable purpose, executor intent, expected output, completion criteria, evidence requirements, self-authority notes, review, and escalation notes in an optional Task execution brief. A separate DelegationPacket binds a frozen brief and source versions to an executor, bounded authority, privacy/context decisions, execution limits, review, and failure/escalation paths. Task lifecycle and planning inputs remain on Task; worker execution leases and statuses remain in the existing assignment contract.\n+\n+Solo briefs are useful without a packet or handoff. A saved self packet is `not_delegated` and creates no lease, notification, or disclosure. Preparing a non-self packet grants no execution authority; dispatch requires current policy/authority checks and explicit parent assignment for workers. Output is candidate evidence until ordinary validation and review admit any resulting canonical changes.\n+\n+Implement only the brief/packet and configured execution paths needed for Phase 1 GitHub dogfooding and worker assignments. Preserve the executor vocabulary without enabling unsupported human/Association handoff, General Contractor subdelegation, or marketplace behavior. DESIGN.md \u00a721.5 defines the required schema, defaults, version/approval bindings, and acceptance fixtures. This resolves the data-model contract, not implementation conformance or those later coordination semantics.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -986,43 +986,43 @@ Open.\n \n ## UBU-Q0079: MCP client/server capability and Compartment policy\n \n Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0028, UBU-Q0032, UBU-Q0060 Blocks: MCP-style integrations, external agents, Delegation Substrate APIs Resolved by: UBU-D0177 Last scored: 2026-05-26 Scored from commit: None\n \n Resolved. See UBU-D0177.\n \n ---\n \n ## UBU-Q0080: Delegation Substrate MVP schema and self-reminder use\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: Task delegation, Automation Worker assignment, solo Task formalization, future Skill Barter marketplace Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: Task delegation, Automation Worker assignment, solo Task formalization, future Skill Barter marketplace Resolved by: UBU-D0342 Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n What is the minimum Delegation Substrate schema that prepares Tasks for delegation while also helping the user perform solo Tasks more clearly?\n \n ### Subquestions\n \n 1. Which fields belong directly on Task versus a separate `DelegationPacket`?\n 2. What executor types are needed: self, local agent, remote/cloud agent, tool, human Identity, Association, General Contractor, or external provider?\n 3. What authority, evidence, privacy, expected-output, review, deadline, and escalation fields are required?\n 4. How should a solo Task use the same structure as a self-reminder without implying actual handoff?\n 5. Which subset is required for Phase 1 GitHub dogfooding and Automation Worker assignments?\n \n ### Current direction\n \n The MVP should implement Delegation Substrate primitives where they support dogfooding, self-reminder clarity, and worker assignment. The full Skill Barter marketplace is not Phase 1.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0342`: DESIGN.md \u00a721.5 specifies optional reusable Task execution briefs, version-bound DelegationPackets, executor types, bounded authority/privacy/context, output/evidence/review rules, execution limits, and failure/escalation paths. Self-reminders imply no handoff; worker dispatch and status reuse \u00a725.1.2. Phase 1 implements only configured dogfooding paths, with GitHub previews and existing review boundaries. Human/Association coordination, General Contractor subdelegation, and marketplace semantics remain future work.\n \n ---\n \n ## UBU-Q0081: General Contractor role and subdelegation semantics\n \n Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 0 Depends on: UBU-Q0080, UBU-Q0061 Blocks: multi-executor coordination, Skill Barter marketplace, larger Association workflows Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n How should UbU model a General Contractor who coordinates multiple human, agentic, tool, or Association executors toward an Objective?\n \n",
    "commit_message": "Define delegation MVP Task briefs and packet contract (UBU-Q0080)",
    "validation_notes": [
      "Generated the diff from current working-tree contents using git diff --no-index, with canonical path headers. git apply --check against the working tree passed; canonical files were not modified.",
      "Verified unique question/decision IDs, unused UBU-D0342, and solved dependencies UBU-Q0032, UBU-Q0060, and UBU-Q0079.",
      "Verified the single OPEN_QUESTIONS.md hunk contains UBU-Q0080's heading and its own Resolution; only that question's status, resolved-by field, and resolution text change. Ranking metadata and neighboring questions are preserved.",
      "Documentation-only proposal. Acceptance fixtures are specified, not executed; implementation conformance and human acceptance remain required."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0080"
    ],
    "decisions_added": [
      "UBU-D0342"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0080-delegation-mvp",
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
