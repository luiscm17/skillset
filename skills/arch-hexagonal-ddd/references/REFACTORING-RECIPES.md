# Evidence-Driven Refactoring Recipes

Select a recipe only after discovery and keep `../SKILL.md` canonical. Each recipe starts with evidence, preserves behavior, and ends at the smallest reversible improvement.

## Isolate Infrastructure Leakage

**Evidence:** A business rule depends on a concrete technical API, or changing infrastructure forces unrelated policy changes.

1. Characterize the business-facing behavior with a focused test.
2. Identify the smallest capability the policy needs.
3. Move translation to the technical edge or introduce a narrow consumer-owned contract.
4. Migrate one path and verify it before expanding.

Stop if the dependency is intentionally technology-specific or the abstraction adds no independence.

## Move a Proven Invariant

**Evidence:** Multiple callers duplicate or bypass the same business rule.

1. Name the invariant and its natural owner.
2. Add tests for accepted and rejected behavior.
3. Move only the decision and required state behind that owner.
4. Leave workflow, persistence, and side-effect coordination where they belong.
5. Remove duplicated checks after all affected paths use the owner.

Do not enrich data-only models without a real invariant.

## Reshape a Boundary

**Evidence:** A module has unrelated change reasons, ownership conflict, an unstable cycle, or consumers must coordinate fragmented contracts.

1. Map callers, dependencies, ownership, and compatibility needs.
2. Choose one cohesive responsibility to extract, merge, or expose.
3. Preserve stable public behavior only where actual consumers require it.
4. Move a narrow consumer slice and verify representative behavior.
5. Remove transitional code when no required consumer remains.

Do not use line count or one-unit-per-file as evidence.

## Introduce or Remove a Port

**Evidence to introduce:** A meaningful volatility, ownership, process, or testability seam exists.

**Evidence to remove:** The contract mirrors one implementation and provides no independent evolution.

1. Define the consumer's required semantics, including errors and absence.
2. Choose interaction style and granularity from runtime needs.
3. Adapt one implementation and migrate consumers incrementally.
4. Trace composition and lifecycle changes.
5. Reassess whether the seam now earns its maintenance cost.

Do not create repositories per entity or interfaces per class by convention.

## Simplify Composition

**Evidence:** Wiring hides dependencies, mishandles lifetimes, repeats inconsistently, or contains factory/container ceremony without benefit.

1. Trace one entry point from configuration through resource cleanup.
2. Make dependencies and ownership visible at the narrowest composition point.
3. Extract shared construction only when repetition causes drift.
4. Remove forwarding factories or containers that add no guarantee.
5. Verify startup, failure, representative execution, and shutdown behavior.

## Change a Context Relationship

**Evidence:** Shared meaning, ownership, or independent evolution has changed.

1. Confirm semantics and owners on both sides.
2. Compare shared ownership, translation, versioned messages, deliberate duplication, and context merge.
3. Migrate one contract or concept with explicit compatibility needs.
4. Verify both local meaning and integration behavior.
5. Define how transitional coupling will be removed.

See [SHARED-KERNEL.md](SHARED-KERNEL.md) for relationship tradeoffs. No recipe implies a full rewrite; widen scope only when evidence shows the change cannot remain coherent otherwise.
