# Application Composition

Composition selects capabilities and implementations, connects public contracts, supplies configuration, and coordinates system-level lifecycle. Composition may depend on capabilities; capabilities must not depend on the composition shell.

## Composition Owns

- application entry points and top-level navigation or flow selection;
- capability assembly and cross-capability wiring;
- environment-specific implementation selection and configuration;
- system-wide lifecycle, registration, and cleanup;
- translation at application edges when no capability owns it.

Composition does not own capability policy, duplicate capability state, or become a global service locator. Cross-capability workflows may be composed at the shell when no single capability owns the product decision; if the workflow has durable semantics and ownership, assign it to an appropriate capability instead.

## Choose Structure From Pressure

| Evidence | Response |
| --- | --- |
| Wiring is small and stable | Keep composition direct and visible. |
| Independent entry points need different graphs | Use separate cohesive composition points. |
| Registration repeats and drifts | Extract the smallest composition helper. |
| A global registry hides dependencies | Make required contracts explicit at the consuming boundary. |
| Shell code accumulates product rules | Move each rule to its canonical capability owner. |
| Capabilities coordinate only through shell-owned state | Prefer public contracts and derive shell views from canonical capability sources. |

Verify representative entry points, implementation selection, lifecycle, failure behavior, and that capability code remains usable without importing composition concerns.
