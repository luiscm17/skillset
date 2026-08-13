# Policy Model

## Boundary And Vocabulary

Authentication supplies a trustworthy subject identity and relevant identity state. RBAC answers whether that subject may perform an action on a resource or scope. Domain/process validation then decides whether an authorized operation is currently valid. Presentation may hide or disable controls but cannot authorize. Audit records decisions and policy changes without becoming a permission source.

Use these generic concepts:

| Concept | Meaning |
| --- | --- |
| Subject | Authenticated identity presented for authorization |
| Role | Reusable collection of policy permissions |
| Preset/template | Starting data copied or linked according to an explicit policy decision |
| Permission | Exact authorization fact, commonly action plus resource/scope |
| Action | Stable identifier for an operation class |
| Resource/scope | Stable identifier for the protected boundary |
| Assignment | Policy fact associating a subject and role |

Roles, presets, permissions, actions, resources, and scopes are catalogs/configuration. Do not encode current job titles, reporting lines, pages, routes, or organizational labels as engine logic. A product may display friendly, localized, or changing labels; authorization compares immutable or migration-stable IDs and exact facts.

## Baseline Evaluation

Start with default deny and least privilege. Identify the requested action and actual resource/scope, resolve only participating assignments and roles, derive effective permissions under the selected composition rules, and allow only a match. Prefer exact action and scope equality. Any wildcard, implication, pattern, or scope containment rule must be explicitly selected, bounded, documented, and tested.

```text
DECIDE(subject_id, action_id, scope_id, policy_snapshot):
    IF subject_id, action_id, or scope_id is unknown or malformed:
        RETURN DENY

    assignments = eligible assignments for subject_id
    roles = eligible roles referenced by assignments
    permissions = COMPOSE(roles, using selected composition semantics)

    IF permissions contains exact (action_id, scope_id):
        RETURN ALLOW

    RETURN DENY
```

Treat this as semantic pseudocode, not a prescribed API or storage algorithm. `eligible`, `COMPOSE`, and exact matching must use the lifecycle and composition choices recorded for the product.

Authentication success grants no permission. Authorization success does not bypass domain invariants. Frontend visibility, URL shape, display text, and navigation state are not authorization evidence.

## Semantic Decision Matrix

| Dimension | Questions to resolve | Safe baseline |
| --- | --- | --- |
| Multiple roles | Can subjects hold several roles? How are grants combined? | No assumption; additive only when selected |
| Hierarchy | Do roles or scopes inherit? How are cycles and depth handled? | No inheritance |
| Explicit deny | Does deny exist? Does it override grant, and at what specificity? | No explicit deny; absence denies |
| Matching | Exact, wildcard, implication, or containment? | Exact action and scope |
| Tenant/context | Is isolation encoded in scope, evaluated as a constraint, or owned elsewhere? | No implicit tenant rule |
| Lifecycle | Which inactive, expired, suspended, or future entities participate? | Only explicitly eligible entities |
| Presets | Copy-on-create or live linkage? | Decide and disclose impact |
| Extensions | Are constraints, ABAC, ReBAC, or session context required? | Keep outside core RBAC unless selected |

Write the chosen decision algorithm before choosing implementation structures. If requirements do not select a row, expose it as unresolved rather than importing a familiar model.

## Ownership And Extension Seams

- The policy owner defines action, resource/scope, role, and preset catalogs.
- The identity owner supplies subject identity and authentication state without becoming authoritative for permissions.
- Each protected capability maps its operations to required stable policy facts and retains its domain validation.
- A trusted policy enforcement point evaluates decisions; presentation consumes advisory capabilities only.
- Administration owns policy mutation, impact analysis, concurrency, invariants, and policy-change audit.
- Optional hierarchy, constraints, tenant isolation, ABAC, or ReBAC should enter through named policy semantics, not accidental conditionals.

Adding catalog entries, labels, scopes, presets, or roles should normally change policy data rather than evaluator code. Engine changes are justified only when semantics change.

## Anti-Patterns

Reject role explosion, one role per person or combination, hardcoded role names, direct authorization from labels/job titles/routes, client-only enforcement, broad or silent wildcards, accidental inheritance, permission implications inferred from naming, and organization-specific namespaces in evaluator logic.

Do not confuse subjects affected by a shared role change with current role members: affected subjects depend on effective-permission deltas, other assignments, lifecycle, and selected composition semantics. Do not fabricate callable operations when an owner/provider contract is missing; omit the control or present an explicit non-interactive unavailable state.
