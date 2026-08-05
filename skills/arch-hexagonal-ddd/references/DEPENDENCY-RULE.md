# Dependency Direction

Use `../SKILL.md` as the canonical policy. Dependency direction protects business policy from concrete infrastructure and allows each side of a meaningful boundary to evolve and be tested independently. It does not require named layers or a fixed hierarchy.

## Analyze Dependencies by Responsibility

For each dependency, identify:

- which side owns the business decision or contract;
- which side contains a replaceable technical detail;
- what change pressure the dependency transmits;
- whether compile-time, runtime, data, or operational coupling is involved;
- whether the boundary provides enough independence to justify its cost.

Source-code imports are useful evidence but not the whole architecture. Generated code, schemas, callbacks, configuration, deployment coupling, shared storage, and runtime lookup can also reverse or bypass an intended boundary.

## Decision Guide

| Evidence | Response |
| --- | --- |
| Business policy imports or exposes a concrete technology | Move the technical decision outward or introduce a business-facing contract at the actual seam. |
| A stable policy depends directly on a volatile collaborator | Invert that dependency when independent replacement or testing matters. |
| Two cohesive modules evolve together and abstraction adds forwarding only | Keep the direct dependency or merge the artificial boundary. |
| A framework controls an entry point | Let the framework own the edge while delegating business decisions to infrastructure-independent code. |
| A shared representation couples independent owners | Translate, version, or assign explicit ownership according to the required relationship. |
| A cycle reflects mutual business responsibility | Revisit ownership and module boundaries before adding an interface mechanically. |

## Verification

Trace representative business behavior from entry point to side effects. Confirm that:

- business rules can be exercised without starting concrete infrastructure;
- changing a technical mechanism does not require rewriting unrelated policy;
- contracts are owned by the side whose needs they express;
- dependency cycles and boundary crossings have an intentional reason;
- architecture tests, import rules, or static analysis reflect the project's actual module model rather than assumed folder names.

Do not infer correctness from a pure unit test alone. A design may still hide data, deployment, or ownership coupling. Report the dependency, the protected quality, the evidence, and the smallest change that improves it.
