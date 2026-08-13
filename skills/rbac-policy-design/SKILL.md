---
name: rbac-policy-design
description: "Trigger: RBAC policy, role design, permission review, access-control review. Design or review RBAC authorization policy; exclude authentication implementation."
license: Apache-2.0
metadata:
  author: "luiscm17"
  version: "1.0.0"
---

# RBAC Policy Design

## Activation Contract

Use for designing, reviewing, or testing RBAC authorization policy. Treat authenticated identity as input. Exclude authentication, credentials, sessions, tokens, identity providers, transports, storage, and framework implementation.

Use one bounded analysis/writer thread. Add at most one read-only specialist only for an independently resolvable standards or threat-model question; forbid checklist fan-out and recursive delegation.

## Hard Rules

- Model roles, presets, permissions, actions, resources, and scopes as policy data, never current job titles, labels, routes, or organizational namespaces.
- Decide with stable IDs and exact policy facts; treat labels and UI visibility as presentation only.
- Default deny and least privilege. Require exact permission matching unless a documented alternative is selected.
- Keep authentication, authorization, domain validation, presentation gating, and audit separate.
- Enforce policy in a trusted backend boundary. Client gating is advisory.
- Do not invent missing provider or owner operations. Omit them or mark them explicitly unavailable and non-interactive.
- Make catalog growth a data change when the selected model permits; do not require authorization-engine changes for new labels, roles, or scopes.

## Decision Gates

| Semantic | Required choice; never assume |
| --- | --- |
| Multiple roles | Additive, single-role, or another defined composition |
| Hierarchy/inheritance | Absent or explicit graph with cycle and precedence rules |
| Deny | Absent or explicit deny with conflict precedence |
| Context | Pure RBAC or explicitly bounded tenant, ABAC, ReBAC, or constraint extension |
| Lifecycle | Which subject, role, assignment, and permission states participate |
| Administration invariants | Whether last-admin or break-glass protection exists |
| Audit reason | Optional, conditionally required, or always required |

## Execution Steps

1. State the protected operation as action plus resource/scope using stable IDs.
2. Discover policy ownership, catalogs, lifecycle, assignment, composition, conflict, and enforcement semantics.
3. Record unresolved choices; do not fill gaps with conventional assumptions.
4. Design decision evaluation, administration safety, extension seams, and audit independently.
5. Review anti-patterns and verify behavior with allow/deny evidence.

## Output Contract

Return the boundary, vocabulary, selected semantics, decision algorithm, administration safeguards, audit facts, extension seams, unresolved decisions, and behavior evidence. Distinguish required policy from recommendations.

## References

- [Policy model](references/policy-model.md)
- [Administration safety](references/administration-safety.md)
- [Verification checklist](references/verification-checklist.md)
