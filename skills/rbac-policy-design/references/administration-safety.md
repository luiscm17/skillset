# Administration Safety

## Impact And Preview

Treat shared policy mutations as changes to every effective decision they influence, not merely edits to one record. Before confirmation, calculate and present:

- the target policy entity and requested delta;
- permissions added and removed;
- subjects whose effective permissions change, distinct from raw role membership;
- lifecycle or composition rules used in the calculation;
- invariants that would pass or fail.

Preview is especially important for shared roles, linked presets, hierarchy edges, wildcard rules, deny precedence, and scope containment. State whether a preset is copied or remains linked. Never imply that editing a template changes existing roles unless that linkage is selected policy.

## Freshness And Conflicts

Bind preview and mutation to a policy version, revision, or equivalent freshness condition. Re-evaluate invariants at commit time. Reject stale writes with enough current state to let the administrator review again.

Do not automatically replay a rejected policy mutation. A replay can apply an old intent to a materially different subject set, effective-permission graph, or invariant state. Require a fresh impact calculation and explicit confirmation.

Use optimistic concurrency unless requirements justify stronger coordination. Define atomicity for mutations that span assignments, roles, presets, or invariants; partial success must not silently create unintended authority.

```text
PREVIEW_AND_APPLY(actor, draft, observed_revision):
    REQUIRE actor is authorized by the current trusted policy
    preview = CALCULATE_IMPACT(draft, observed_revision)
    PRESENT preview and request explicit confirmation

    IF draft, subject, policy, or revision changed before confirmation:
        INVALIDATE preview
        REQUIRE a fresh preview

    result = APPLY_ONCE(draft, expected_revision = preview.revision)

    IF result is conflict or an invariant rejects the change:
        DO NOT replay
        LOAD current policy state
        REQUIRE administrator review and a fresh preview

    AUDIT only the committed mutation
```

This sequence expresses safety properties, not a required interface, transaction mechanism, or user-interface shape.

## Core Administration Invariants

Last-administrator and break-glass protections are valuable only when the product defines their meaning. Do not invent a universal administrator role, reserved name, hidden superuser, or permanent bypass.

When selected, specify:

- the stable permission facts that qualify an administrator;
- which identity and policy lifecycle states count;
- whether the invariant concerns assigned or effective permissions;
- initialization, recovery, rotation, monitoring, and revocation rules;
- whether emergency access bypasses normal policy and how that use is reviewed.

Define revocation effective time: when new decisions must deny, how cached allows are invalidated, and how in-flight or delayed operations are handled. A successful policy write is not sufficient if enforcement points may continue granting beyond the stated bound.

Treat capability or bundle rename, split, merge, deprecation, and membership changes as migrations, not label edits. Preview old-to-new mappings and effective authority deltas; version bundles; preserve stable audit lineage; reject partial, mixed-version, or ambiguous conversion; and provide a verified rollback or forward-correction plan.

## Audit Policy

Record policy changes with actor, subject or policy target, change type, before state, after state, and time. Include policy version and correlation context when available. Keep authentication secrets and unrelated personal data out of policy audit.

Choose reason handling explicitly: optional, required for selected high-impact changes, or required for every change. Do not declare reasons universally mandatory. Preserve stable IDs with labels only as contextual snapshots; labels must not become decision facts.

Define audit access, retention, integrity, export, and review ownership. Audit should support reconstruction of effective-policy changes without exposing more subject information than reviewers need.

## Optional Human-Facing Guidance

Apply only when the system has human-facing administration; do not infer or prescribe an interface.

- Describe consequences in plain language and expose exact policy facts for verification.
- Do not rely on color, iconography, pointer interaction, or visual placement alone to communicate risk or state.
- Make confirmation deliberate and reversible where policy permits; identify irreversible consequences.
- If unavailable operations are represented, make their status and reason unambiguous; otherwise omit them.
- Do not render a functional-looking placeholder for a missing owner/provider contract.
- Limit previews and audit views to authorized viewers and necessary subject data.
- Provide equivalent accessible understanding and operation without making presentation behavior part of policy semantics.
