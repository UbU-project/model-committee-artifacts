# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0060`  
Question title: `External/cloud LLM provider abstraction and routing policy`  
Base commit: `5ebbb1d00b97d4513ca0c3289991ee01b5fd313e`

```markdown
## UBU-Q0060: External/cloud LLM provider abstraction and routing policy

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028 Blocks: LLM provider routing, cloud LLM disclosure, UbUCorp inference boundary, BYOK configuration Resolved by: None Last scored: Never Scored from commit: None

### Question

What is the minimum provider-neutral LLM routing model that supports local Ollama, user-configured BYOK cloud APIs, optional UbUCorp managed inference, user-owned remote workers, and future compatible providers while preserving Compartment policy and user sovereignty?

### Subquestions

1. What interface should normalize local and cloud model providers?
2. What provider metadata is required for location, cost, context window, capability, retention/disclosure profile, and safety behavior?
3. How should Compartment policy prevent `no_cloud_llm` or `no_external_export` payloads from crossing a provider boundary?
4. What context-minimization, redaction, and provenance fields are required before cloud routing?
5. How should BYOK mode store and scope provider credentials?
6. How should UbUCorp managed inference be represented without becoming a mandatory dependency of the open core?
7. What user-visible disclosure is required before a workflow uses a cloud LLM?

### Current direction

Cloud LLMs are optional execution providers, not the canonical planner. The FOSS core should remain local-capable, provider-neutral, BYOK-capable, and self-hostable. Cloud routing must be policy-governed, explicit, and advisory.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0060-001",
    "provider_id": "codex",
    "model_name": "gpt-5",
    "question_id": "UBU-Q0060",
    "base_commit": "5ebbb1d00b97d4513ca0c3289991ee01b5fd313e",
    "summary": "Resolve UBU-Q0060 by adding a minimum provider-neutral LLM routing model with provider descriptors, route requests/decisions, Compartment hard gates, BYOK credential scoping, UbUCorp managed-inference boundaries, and cloud-use disclosure requirements.",
    "rationale": "UBU-Q0028 is already solved, so the selected question is answerable without decomposition. The proposal keeps the open core local-capable and provider-neutral while making cloud and external LLM use explicit, policy-gated, auditable, and advisory. It also unblocks dependent MCP, Delegation Substrate, ContextBundle, and background-agent questions without over-specifying provider SDK details.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -223,6 +223,22 @@\n The core model should support provider-neutral routing across local model providers, user-configured BYOK cloud APIs, user-owned remote workers, optional UbUCorp managed gateways, and future compatible providers. External/cloud routing must remain governed by Compartment policy, context minimization, redaction where appropriate, cost policy, provenance, and user-visible disclosure.\n \n Local-first operation remains the philosophical baseline. Cloud LLM use may improve performance and scalability, but it must not become a hidden dependency of the FOSS core or a way to bypass `no_cloud_llm` Compartments.\n+\n+The minimum provider-neutral model is an `LLMProviderDescriptor` plus a routed invocation envelope. Provider descriptors identify `provider_id`, `provider_class`, `endpoint_or_worker_ref`, execution location, operator, region or jurisdiction when known, supported models, modalities, context window, structured-output and tool-use capabilities, safety behavior, retention/training/disclosure profile, cost and rate-limit metadata, credential reference kind, default-enabled state, and user-visible name. Provider classes are `local_process`, `byok_cloud_api`, `user_owned_worker`, `ubucorp_managed_gateway`, and `third_party_compatible`.\n+\n+Provider adapters should expose the same narrow interface regardless of backend: list available models and capabilities, estimate cost and context fit, prepare a minimized request, invoke or stream completion, cancel when supported, and return usage, model, provider, and provenance metadata. The adapter interface normalizes execution; it does not erase trust-boundary differences.\n+\n+An `LLMRouteRequest` records purpose, actor Identity, related Task/Objective/workflow refs, required capabilities, candidate provider policy, cost and latency limits, ContextBundle or source refs, required output schema when any, review requirement, and requested advisory use. An `LLMRouteDecision` records the selected provider and model, local/internal/external/cloud boundary classification, Compartment policy result, redaction and minimization summary, estimated cost, user-disclosure text, approval state, and Log refs for allowed or denied routing.\n+\n+Compartment policy is evaluated before context assembly and again before provider invocation. Content from a `no_cloud_llm` Compartment cannot be sent to cloud provider classes, including BYOK cloud APIs, UbUCorp managed gateways, or third-party compatible cloud providers. Content from a `no_external_export` Compartment cannot be sent to external providers, remote workers, managed gateways, or third-party services except as redacted structural references that expose no protected payload. `local_only` content is limited to eligible local Devices and local providers. A capability grant, provider setting, or user approval cannot override these hard Compartment denials.\n+\n+Cloud routing requires a minimized `ContextBundle` or equivalent envelope that records purpose, source object refs, Compartment refs, Identity and Association refs exposed, destination provider, model ref, data categories exposed, redaction policy, minimization notes, retention policy, user-visible summary, creation time, expiry when applicable, and downstream candidate refs. Raw payloads should be replaced with references, summaries, hashes, or redacted structural fields whenever the task can still be performed.\n+\n+BYOK credentials are stored as scoped credential references, not embedded in provider descriptors, ContextBundles, prompts, Logs, or exported artifacts. A credential may be scoped by provider, account, model family, Identity, Compartment, workflow, cost budget, and Device or worker. The default storage target is the local instance or OS/device secret store; a user-owned worker may hold its own credential if explicitly configured. Credential rotation and revocation preserve audit continuity through credential version refs without exposing the secret.\n+\n+UbUCorp managed inference is represented as one provider class, not as privileged protocol authority. It must use the same provider descriptor, routing envelope, Compartment gates, disclosure, cost controls, provenance, and output admission path as any other cloud provider. The FOSS core must remain useful when the UbUCorp provider is absent.\n+\n+Before or at execution time, any workflow that uses a cloud or external LLM must disclose the provider, model or model class, operator, local-vs-cloud status, destination region when known, whether BYOK or UbUCorp-managed credentials are used, data categories and Compartments exposed, retention/training profile, estimated cost when available, and the fact that output is advisory until admitted through UbU validation and review. Users may grant per-run, session, workflow, or policy-based approval only when Compartment policy allows it.\n \n ---\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -2757,3 +2757,37 @@\n Behavioral-risk checks such as manipulation, coercion, harassment, stalking, deception, romantic/professional power asymmetry, rumination, and relationship-transition ethics depend on fallible classification. Treating them as hard gates risks false positives that block legitimate actions, false negatives that create misplaced trust, and a false impression that UbU can reliably prevent misuse or illegal behavior.\n \n Behavioral safeguards should generally be default-on, user-aware, uncertainty-transparent, user-overrideable advisory checks with introspection consequences when bypassed. A user may configure some advisory checks as self-imposed required gates. Bypassing or disabling recommended safeguards is itself introspection-relevant evidence about revealed priorities and possible conflicts with declared values.\n+\n+\n+---\n+\n+## UBU-D0157: LLM provider routing uses provider descriptors and policy-gated route decisions\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a72.11\n+\n+Resolved question: `UBU-Q0060`.\n+\n+UbU resolves external/cloud LLM routing through a provider-neutral descriptor plus a policy-gated route decision. Local Ollama-style providers, user-configured BYOK cloud APIs, user-owned remote workers, optional UbUCorp managed inference, and future compatible providers are all represented as LLM providers, but their trust boundaries remain explicit.\n+\n+The minimum `LLMProviderDescriptor` records `provider_id`, `provider_class`, `endpoint_or_worker_ref`, execution location, operator, region or jurisdiction when known, supported models, modalities, context window, structured-output and tool-use capability, safety behavior, retention/training/disclosure profile, cost and rate-limit metadata, credential reference kind, default-enabled state, and user-visible name. The Phase 1 provider classes are `local_process`, `byok_cloud_api`, `user_owned_worker`, `ubucorp_managed_gateway`, and `third_party_compatible`.\n+\n+Every provider adapter exposes the same narrow interface: list available models and capabilities, estimate cost and context fit, prepare a minimized request, invoke or stream completion, cancel when supported, and return usage, provider, model, and provenance metadata. The adapter interface normalizes execution mechanics only. It must not hide whether a request stays local, goes to a user-owned worker, or crosses a cloud/provider boundary.\n+\n+An `LLMRouteRequest` records purpose, actor Identity, related Task/Objective/workflow refs, required capabilities, candidate provider policy, cost and latency limits, ContextBundle or source refs, required output schema when any, review requirement, and requested advisory use. An `LLMRouteDecision` records selected provider and model, boundary classification, Compartment policy result, minimization and redaction summary, estimated cost, disclosure text, approval state, and Log refs for allowed or denied routing.\n+\n+Compartment policy is a hard upper bound on routing. `no_cloud_llm` content cannot be sent to cloud provider classes, including BYOK cloud APIs, UbUCorp managed gateways, or third-party compatible cloud providers. `no_external_export` content cannot be sent to external providers, remote workers, managed gateways, or third-party services except as redacted structural references that expose no protected payload. `local_only` content is limited to eligible local Devices and local providers. A capability grant, provider preference, workflow setting, or user click cannot override these denials.\n+\n+Cloud routing requires context minimization before invocation. The route should use a `ContextBundle` or equivalent envelope that records purpose, source refs, Compartment refs, Identity and Association refs exposed, destination provider, model ref, data categories exposed, redaction policy, minimization notes, retention policy, user-visible summary, creation time, expiry when applicable, and downstream candidate refs. Raw payload should be replaced by references, summaries, hashes, or redacted structural fields whenever the task can still be performed.\n+\n+BYOK credentials are credential references, not provider metadata payload. Secrets must not appear in provider descriptors, prompts, ContextBundles, Logs, or exported artifacts. Credential references may be scoped by provider, account, model family, Identity, Compartment, workflow, cost budget, and Device or worker. The default storage target is the local instance or OS/device secret store; a user-owned worker may hold a credential only when explicitly configured. Rotation and revocation preserve audit continuity through credential version refs without exposing the secret.\n+\n+UbUCorp managed inference is represented as a provider class, not as a mandatory dependency or privileged protocol path. It uses the same descriptor, route request, route decision, Compartment gates, disclosure, provenance, cost controls, and output admission pipeline as any other cloud provider. The FOSS core must remain local-capable, BYOK-capable, self-hostable, and useful when UbUCorp managed inference is unavailable.\n+\n+Any workflow using a cloud or external LLM must disclose, before or at execution time, the provider, model or model class, operator, local-vs-cloud status, destination region when known, whether BYOK or UbUCorp-managed credentials are used, data categories and Compartments exposed, retention/training profile, estimated cost when available, and that output is advisory until admitted through UbU validation and review. Approval may be per-run, per-session, per-workflow, or policy-based only when Compartment policy allows it.\n+\n+**Consequences:**\n+\n+- `UBU-Q0060` is resolved for Phase 1 architecture.\n+- LLM routing can proceed without privileging Ollama, UbUCorp, or any single cloud API as the canonical interface.\n+- `UBU-Q0079`, `UBU-Q0080`, `UBU-Q0083`, and `UBU-Q0084` may depend on this routing boundary while still refining MCP tools, Delegation Substrate fields, ContextBundle governance, and background-agent policy.\n+- Provider-specific SDK details, exact secret-store implementation, and long-context bundle review UX may evolve without changing the minimum boundary.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1142,83 +1142,83 @@\n Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016 Blocks: Phase 2 personal worker devices, hosted compute policy, third-party provider compatibility Resolved by: None Last scored: Never Scored from commit: None\n \n ### Question\n \n How should UbU define the trust, privacy, capability, API, and disclosure boundary for execution backends beyond the current local instance?\n \n ### Subquestions\n \n 1. What is the minimum Phase 2 personal worker protocol for a user-owned laptop, desktop, or server?\n 2. How should the planner route work among mobile, local desktop/laptop, personal worker, corporate cloud, and third-party provider backends?\n 3. What Compartment rules, PII stripping, encryption, redaction, and provenance are required before any external backend receives work?\n 4. What must be open-core so FOSS contributors can inspect, run, modify, and self-host the planning loop without a hidden service?\n 5. How should third-party UbU-compatible providers interoperate without being privileged over local execution?\n 6. Should dedicated personal worker appliances be treated as commercial packaging, reference hardware, or future product work?\n 7. How should future practical FHE-backed or comparable encrypted compute be prioritized if it becomes viable?\n 8. Should idle compute monetization be an external API-enabled user choice rather than an UbU corporate marketplace?\n \n ### Current direction\n \n Cloud or external compute may improve performance, granularity, and analysis depth, but must not be a hidden mandatory dependency for the open-core planning loop. User-owned worker devices are Phase 2. Dedicated appliances, corporate cloud, third-party providers, compute monetization, and FHE/private encrypted compute are future commercial or strategic-research directions.\n \n ### Resolution\n \n Open.\n \n \n ---\n \n ## UBU-Q0060: External/cloud LLM provider abstraction and routing policy\n \n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028 Blocks: LLM provider routing, cloud LLM disclosure, UbUCorp inference boundary, BYOK configuration Resolved by: None Last scored: Never Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028 Blocks: LLM provider routing, cloud LLM disclosure, UbUCorp inference boundary, BYOK configuration Resolved by: UBU-D0157 Last scored: Never Scored from commit: None\n \n ### Question\n \n What is the minimum provider-neutral LLM routing model that supports local Ollama, user-configured BYOK cloud APIs, optional UbUCorp managed inference, user-owned remote workers, and future compatible providers while preserving Compartment policy and user sovereignty?\n \n ### Subquestions\n \n 1. What interface should normalize local and cloud model providers?\n 2. What provider metadata is required for location, cost, context window, capability, retention/disclosure profile, and safety behavior?\n 3. How should Compartment policy prevent `no_cloud_llm` or `no_external_export` payloads from crossing a provider boundary?\n 4. What context-minimization, redaction, and provenance fields are required before cloud routing?\n 5. How should BYOK mode store and scope provider credentials?\n 6. How should UbUCorp managed inference be represented without becoming a mandatory dependency of the open core?\n 7. What user-visible disclosure is required before a workflow uses a cloud LLM?\n \n ### Current direction\n \n Cloud LLMs are optional execution providers, not the canonical planner. The FOSS core should remain local-capable, provider-neutral, BYOK-capable, and self-hostable. Cloud routing must be policy-governed, explicit, and advisory.\n \n ### Resolution\n \n-Open.\n+Resolved. See UBU-D0157.\n \n ---\n \n ## UBU-Q0061: Association object model and lifecycle\n \n Status: Open Priority: Post-MVP Phase: Phase 3 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0054, UBU-Q0055, UBU-Q0056 Blocks: Phase 3 multi-user coordination, Association reconciliation, future skill-barter systems Resolved by: None Last scored: Never Scored from commit: None\n \n ### Question\n \n Should UbU introduce a first-class Association object, and if so what minimum fields describe an Identity-scoped perceived coordination structure without pretending that membership, authority, or boundaries are globally objective?\n \n ### Subquestions\n \n 1. What distinguishes an Association from an Organization Identity, Relationship, or External Reference?\n 2. Can an Association exist entirely inside one user's `user_mode` model?\n 3. What minimum fields describe perceived members, roles, shared Objectives, commitments, lifecycle, norms, confidence, and disclosure policy?\n 4. How should UbU represent informal groups such as friend groups, parties, amateur leagues, FOSS projects, conference cohorts, and skill networks?\n 5. How does an Association become formal enough to justify `organization_mode`?\n 6. How are invitations, exits, revocations, dormancy, and dissolution represented?\n 7. Which parts are required for Phase 3, and which remain later research?\n \n ### Current direction\n \n An Association is an Identity-scoped, perspective-bound model of emergent coordination. Legal entities and institutional records are External References or evidence, not the whole Association.\n \n ### Resolution\n \n Open.\n \n ---\n",
    "commit_message": "Resolve LLM provider routing policy\n\nAdd UBU-D0157, expand the Cloud LLM provider boundary in DESIGN.md, and mark UBU-Q0060 solved.",
    "validation_notes": [
      "Patch applies cleanly with git apply --check against the provided workspace state.",
      "Patch modifies only DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md.",
      "OPEN_QUESTIONS.md updates are anchored inside the UBU-Q0060 block and preserve the single-line metadata format."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0060"
    ],
    "decisions_added": [
      "UBU-D0157"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0060-001",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
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
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
