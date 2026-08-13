# Policy Model

## Boundary And Vocabulary

Identity proofing or another trusted mechanism supplies a principal and relevant context when the policy requires identity. Authorization answers whether that principal may exercise a capability on a target. Domain validation decides whether an authorized operation is currently valid. Presentation may advise but cannot authorize. Audit records decisions and policy changes without becoming a permission source.

Use these generic concepts:

| Concept | Meaning |
| --- | --- |
| Subject | Entity whose access is being decided |
| Principal | Trusted identity acting for itself or a subject; may be anonymous, human, workload, service, device, or delegated when selected |
| Role | Reusable collection of policy permissions |
| Preset/template | Starting data copied or linked according to an explicit policy decision |
| Permission/capability | Stable domain operation authorized against a target |
| Resource type | Stable `resource_type_id` identifying the target class |
| Resource instance | Optional `resource_instance_id` identifying one exact target |
| Scope | Optional `scope_ref` defining a tenant, partition, or bounded authority |
| Assignment | Policy fact associating a subject or principal with a role |
| Trusted context | Verified attributes used by selected constraints; untrusted or missing values cannot grant |
| Policy snapshot | Immutable or freshness-bounded facts identified by a policy version |

Roles, presets, capabilities, resources, and scopes are policy catalogs. Interfaces, protocols, jobs, and presentation controls adapt operations to canonical capabilities; they are not policy semantics. Authorization compares immutable or migration-stable IDs and exact facts.

## Action Taxonomy

Use layered names deliberately:

- Stable domain capabilities are canonical because they express business authority without coupling policy to an entry point.
- Generic CRUD capabilities are canonical only where create/read/update/delete exactly describe the domain authority.
- Read/write are useful coarse categories for analysis or deliberately coarse policy, but must not silently replace precise capabilities.
- Bundles such as `manage` are explicit, versioned sets of capability IDs. Expansion occurs from policy data before matching.

Never infer hierarchy from separators, prefixes, or similar strings. Wildcards and bundles must not silently include future capabilities. Bundle versions require impact review because a changed member set changes authority.

## Baseline Evaluation

Start with default deny and least privilege. Identify the requested action and actual resource/scope, resolve only participating assignments and roles, derive effective permissions under the selected composition rules, and allow only a match. Prefer exact action and scope equality. Any wildcard, implication, pattern, or scope containment rule must be explicitly selected, bounded, documented, and tested.

```text
EXPAND(bundle_ref, policy_snapshot):
    RETURN the exact capability IDs recorded for bundle_ref.version

DECIDE(principal, subject, capability_id, resource_type_id,
       resource_instance_id?, scope_ref?, trusted_context, policy_snapshot):
    IF required identity or any required ID/context is missing, untrusted, or malformed:
        RETURN DENY

    assignments = eligible assignments for the selected subject/principal semantics
    roles = roles eligible under assignment and activation rules
    permissions = explicit direct grants if selected
                + capabilities from roles and explicitly expanded bundles

    FOR permission IN COMPOSE(permissions, using selected conflict semantics):
        IF permission exactly matches capability_id and resource_type_id
           AND optional instance and scope requirements exactly match
           AND all selected constraints and SoD rules pass:
            RETURN ALLOW with matched facts and policy_snapshot.version

    RETURN DENY with reason and policy_snapshot.version
```

Treat this as semantic pseudocode, not a prescribed API or storage algorithm. Role assignment makes a role available; activation makes it participate in a decision. A policy may use assignment alone or require activation. Permissions flow through roles by default; direct grants are a separate explicit choice with lifecycle, conflict, and audit rules.

Successful identity verification grants no permission. Authorization success does not bypass domain invariants. Presentation visibility, locator shape, display text, and navigation state are not authorization evidence.

## Semantic Decision Matrix

| Dimension | Questions to resolve | Safe baseline |
| --- | --- | --- |
| Multiple roles | Can subjects hold several roles? How are grants combined? | No assumption; additive only when selected |
| Hierarchy | Do roles or scopes inherit? How are cycles and depth handled? | No inheritance |
| Explicit deny | Does deny exist? Does it override grant, and at what specificity? | No explicit deny; absence denies |
| Matching | Exact capability/type/instance/scope, explicit bundle, wildcard, implication, or containment? | Exact facts |
| Target | Type-wide or instance-specific permission? | No guessed instance or implicit widening |
| Tenant/context | Is isolation encoded in scope, evaluated as a constraint, or owned elsewhere? | No implicit cross-scope access |
| Activation/direct grants | Do assigned roles require activation? Are direct grants allowed? | Role-derived permissions; decide activation |
| SoD | Which role combinations or operations conflict statically or dynamically? | No invented constraint |
| Lifecycle | Which inactive, expired, suspended, or future entities participate? | Only explicitly eligible entities |
| Presets | Copy-on-create or live linkage? | Decide and disclose impact |
| Extensions | Are constraints, ABAC, ReBAC, or session context required? | Keep outside core RBAC unless selected |

Write the chosen decision algorithm before choosing implementation structures. If requirements do not select a row, expose it as unresolved rather than importing a familiar model.

## Extension Classification

- Core RBAC: role assignment and permission through roles.
- Hierarchical RBAC: explicit role inheritance with cycle, depth, and precedence rules.
- Constrained RBAC: static or dynamic separation of duties and other role constraints.
- ABAC: decisions depend on trusted subject, target, action, or environment attributes.
- ReBAC/ownership: decisions depend on verified relationships; ownership is not an implicit bypass.
- Delegation/impersonation: one principal acts for a subject under explicit provenance, limits, and expiry.
- Operational enforcement: policy enforcement points, caching, revocation, audit, and check-to-use controls; these support but do not redefine RBAC.

## Ownership And Evolution

- The policy owner defines action, resource/scope, role, and preset catalogs.
- The identity/context owner supplies trusted principal, subject, delegation, and attributes without becoming authoritative for permissions.
- Each protected capability maps its operations to required stable policy facts and retains its domain validation.
- A trusted policy enforcement point evaluates decisions; presentation consumes advisory capabilities only.
- Administration owns policy mutation, impact analysis, concurrency, invariants, and policy-change audit.
- Optional hierarchy, constraints, tenant isolation, ABAC, or ReBAC should enter through named policy semantics, not accidental conditionals.

Adding catalog entries, labels, scopes, presets, or roles should normally change policy data rather than evaluator code. Engine changes are justified only when semantics change.

Bind decisions to a policy snapshot/version. Define cache lifetime, invalidation, revocation effective time, and whether sensitive mutations require a fresh decision or atomic coupling to prevent time-of-check/time-of-use widening. Fail closed when required context or freshness cannot be established.

Treat capability and bundle rename, split, merge, deprecation, and changed semantics as explicit migrations. Map old facts to reviewed new facts, preview authority deltas, preserve audit lineage, version bundles, and prevent mixed-version evaluation from granting unintended access.

## Anti-Patterns

Reject role explosion, one role per person or combination, hardcoded role names, authorization from labels or entry-point names, presentation-only enforcement, broad or silent wildcards, accidental inheritance, permission implications inferred from naming, guessed resource instances, and hidden owner or administrator bypasses.

Do not confuse subjects affected by a shared role change with current role members: affected subjects depend on effective-permission deltas, other assignments, lifecycle, and selected composition semantics. Do not fabricate callable operations when an owner/provider contract is missing; omit the control or present an explicit non-interactive unavailable state.
