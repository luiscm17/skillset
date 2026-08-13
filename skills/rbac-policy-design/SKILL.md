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

Use for designing, reviewing, or testing RBAC authorization policy. When authorization requires identity, treat trusted subject/principal identity and trusted context as inputs. Support selected anonymous, human, workload, service, device, or delegated principals without forcing every kind into scope. Exclude identity proofing, credentials, transports, storage, and framework implementation.

Use one bounded analysis/writer thread. Add at most one read-only specialist only for an independently resolvable standards or threat-model question; forbid checklist fan-out and recursive delegation.

## Hard Rules

- Model roles, presets, capabilities, resources, and scopes as policy data, never current job titles, labels, routes, interfaces, jobs, or organizational namespaces.
- Decide with stable IDs and exact policy facts; treat labels and UI visibility as presentation only.
- Model canonical actions as stable domain capabilities. Use generic CRUD only when semantically exact; treat read/write as coarse categories, not required actions. Map interfaces and execution entry points through adapters.
- Default deny and least privilege. Require exact capability and resource matching unless a documented alternative is selected.
- Keep authentication, authorization, domain validation, presentation gating, and audit separate.
- Enforce policy at a trusted policy enforcement point. Presentation gating is advisory.
- Do not invent missing provider or owner operations. Omit them or mark them explicitly unavailable and non-interactive.
- Make catalog growth a data change when the selected model permits; do not require authorization-engine changes for new labels, roles, or scopes.

## Decision Gates

| Semantic | Required choice; never assume |
| --- | --- |
| Principal | Kinds admitted; anonymous behavior; delegation/impersonation semantics |
| Target | Resource type alone or exact instance; relationship to scope/tenant |
| Multiple roles | Additive, single-role, or another defined composition |
| Role participation | Assigned roles or only active roles; activation conditions |
| Grants | Permissions through roles only or explicit direct grants |
| Hierarchy/inheritance | Absent or explicit graph with cycle and precedence rules |
| Deny | Absent or explicit deny with conflict precedence |
| Constraints | Static/dynamic SoD and trusted attributes; failure behavior |
| Extensions | Classify Core, hierarchical, constrained RBAC, ABAC, ReBAC, delegation, and operational enforcement |
| Freshness | Cache bounds, revocation effective time, and check-to-use guarantees |
| Evolution | Snapshot/version evidence and rename, bundle, or semantic migration rules |
| Lifecycle | Which subject, role, assignment, and permission states participate |
| Administration invariants | Whether last-admin or break-glass protection exists |
| Audit reason | Optional, conditionally required, or always required |

## Execution Steps

1. State the protected operation as capability, resource type, optional instance, and optional scope using stable IDs.
2. Discover policy ownership, catalogs, lifecycle, assignment, composition, conflict, and enforcement semantics.
3. Record unresolved choices; do not fill gaps with conventional assumptions.
4. Design decision evaluation, administration safety, extension seams, and audit independently.
5. Review anti-patterns and verify behavior with allow/deny evidence.

## Output Contract

Return the boundary, vocabulary, semantic decision table, decision algorithm, administration safeguards, audit facts, extension classification, unresolved decisions, and allow/deny evidence tied to stable policy facts and policy version. Distinguish required policy from recommendations.

## References

- [Policy model](references/policy-model.md)
- [Administration safety](references/administration-safety.md)
- [Verification checklist](references/verification-checklist.md)
