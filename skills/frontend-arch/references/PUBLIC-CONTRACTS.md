# Capability Public Contracts

A capability's public contract is its narrow, stable promise to consumers. It may be expressed through functions, types, messages, commands, queries, events, factories, or another ecosystem-native mechanism. It is not required to be one file or one interface.

## Contract Content

Expose only what consumers need:

- capability operations and observable outcomes;
- consumer-relevant state or projections;
- success, absence, rejection, and failure semantics;
- timing, ordering, cancellation, and invalidation when observable;
- compatibility and evolution guarantees that real consumers require.

Hide internal state shape, orchestration steps, technical dependencies, private helpers, and transitive exports. A public type that reveals internal representation is still an internal dependency in disguise.

## Ownership and Granularity

The providing capability owns its contract while consumers supply evidence about required semantics. Prefer one cohesive surface over many fragments that force consumers to reconstruct a workflow. Split when permissions, lifecycle, performance, ownership, or evolution differ materially.

| Situation | Guidance |
| --- | --- |
| One consumer needs a stable semantic operation | Expose that operation, not the provider's internal machinery. |
| Consumers need different representations | Publish consumer-relevant projections or let consumers translate from a stable semantic result. |
| A contract mirrors implementation details | Redesign around observable capability meaning. |
| Compatibility has no external requirement | Prefer a coherent direct change over permanent aliases. |
| Existing consumers need a transition | Add the narrowest time-bounded compatibility surface and define removal proof. |

## Public Entry Points

Use the ecosystem's strongest practical mechanism to distinguish public from internal code: explicit exports, package boundaries, visibility rules, dependency checks, or documented entry points backed by review. Avoid broad export barrels that accidentally publish internals.

Review contracts for necessity, semantic precision, owner, consumers, compatibility burden, and the ability to change internals without coordinated edits.
