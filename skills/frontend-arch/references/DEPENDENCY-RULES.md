# Capability Dependency Rules

Use `../SKILL.md` as canonical policy. Dependencies must preserve capability ownership and independent evolution; directory depth alone proves nothing.

## Allowed Direction

```text
composition -> capability public contracts
capability internals -> their own public vocabulary and required technical seams
capability A -> capability B public contract
shared technical mechanisms <- explicit capability use
```

Forbidden directions include capability internals reaching into another capability's internals, any capability depending on the composition shell, and shared mechanisms depending on capability-specific policy.

## Analyze Every Crossing

Identify the owner of meaning, the consumer need, the exposed stability promise, and the coupling transmitted by imports, callbacks, messages, schemas, state access, registration, or runtime lookup. Source imports are evidence, not the entire dependency model.

| Evidence | Response |
| --- | --- |
| Consumer reaches through another capability's public surface | Add or reshape the provider-owned semantic contract, then migrate the consumer. |
| Two capabilities require each other's internals | Revisit ownership, extract explicit collaboration, or merge an artificial boundary. |
| Composition decisions leak into capabilities | Move selection and wiring outward; pass only required collaborators or values inward. |
| Shared code contains capability policy | Return policy to its canonical capability owner. |
| A direct dependency is cohesive and stable | Keep it; do not add an interface only for symmetry. |
| A volatile integration affects stable policy | Introduce the smallest consumer-shaped seam that protects independent change. |

## Verification

Trace representative behavior from composition through public contracts to effects. Verify forbidden imports, runtime lookups, global state, generated bindings, and shared representations as applicable. Report the violated invariant, concrete cost, smallest correction, and residual coupling.
