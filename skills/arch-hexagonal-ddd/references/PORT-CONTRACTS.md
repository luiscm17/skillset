# Port Contracts

Use `../SKILL.md` as the canonical policy. A port is useful when a business-facing capability crosses a meaningful ownership, volatility, process, or technology seam. It is not required for every class, dependency, entity, or data operation.

## Introduce a Port When It Earns Its Cost

Consider a port when the caller needs a stable capability while implementations vary, infrastructure replacement matters, independent testing needs a controllable seam, or another owner supplies the behavior. Keep a direct dependency when both sides evolve together and an abstraction would only mirror one implementation.

Define the contract from consumer needs rather than copying a vendor API. A port can be an interface, protocol, function, message, schema, callback, or another ecosystem-native contract.

## Contract Semantics

Decide and document only what consumers need:

- operation meaning, preconditions, and observable effects;
- success, absence, rejection, retry, and failure semantics;
- consistency, ordering, idempotency, and timeout expectations where relevant;
- synchronous, asynchronous, streaming, or message-based interaction;
- cancellation, pagination, batching, and backpressure when demanded by scale;
- type precision and compatibility guarantees supported by the project;
- ownership, versioning, and evolution policy.

Do not force one universal choice for nullable results, exceptions, result types, error taxonomies, strict typing, or read-model placement. Do not expose concrete infrastructure types when doing so makes business policy depend on that infrastructure. Boundary-specific representations are valid when they preserve meaning and reduce coupling.

## Granularity

Prefer a cohesive capability contract. Split when consumers need independent evolution, permissions, performance characteristics, lifecycle, or ownership. Merge contracts that always change together or require clients to coordinate fragments of one operation.

Repositories are one possible persistence port. Introduce them around a meaningful consistency or ownership boundary, not per entity by rule. Their methods should reflect required semantics; `save`, create/update operations, queries, and unit-of-work behavior are contextual choices.

## Review Questions

- What concrete seam or change pressure justifies this port?
- Whose needs define the contract?
- Does it hide volatility or merely rename an implementation API?
- Are absence and errors unambiguous to all consumers?
- Does interaction style match runtime and consistency needs?
- Is the contract cohesive, minimal, and evolvable?
- Would removing it reduce indirection without sacrificing independence or testability?
