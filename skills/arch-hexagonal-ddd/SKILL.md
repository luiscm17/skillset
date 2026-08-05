---
name: arch-hexagonal-ddd
description: "Trigger: explicit Hexagonal Architecture, DDD, SOLID, or design-pattern work. Improve boundaries without imposing project structure."
license: MIT
metadata:
  author: luiscm17
  version: "2.0.0"
---

# Hexagonal Architecture and DDD

## Activation Contract

Use only for explicit Hexagonal Architecture, DDD, SOLID, design-pattern, architecture migration, or architecture audit work. Do not activate for generic typing, namespace, file-layout, or cleanup work unless it belongs to that explicit architecture scope.

## Hard Rules

- Discover the language, framework, current architecture, tests, compatibility constraints, and requested scope before prescribing structure.
- Preserve inward dependency direction and isolate business policy from infrastructure details.
- Evaluate architecture through responsibilities, boundaries, dependency direction, cohesion, coupling, and change pressure—not prescribed folders, namespaces, class names, or framework conventions.
- Apply SOLID and design patterns proportionally to observed problems. Never introduce interfaces, layers, factories, repositories, or patterns only for theoretical purity.
- Preserve the project's language and ecosystem idioms unless they violate a selected core invariant.
- Allow modules and file boundaries to split, merge, or reorganize as ownership, team topology, scale, and change frequency evolve.
- Treat the policy table below as canonical; references provide supporting guidance, not stronger rules.
- Delegate to at most three workers at depth one. Never delegate by checklist item, file, layer, or reference. Architecture analysis defaults to one worker.
- Load at most two references by default. A full audit may read all references sequentially, then MUST produce one deduplicated plan before any work.

## Decision Gates

| Level | Policies |
| --- | --- |
| Core invariants | Dependencies point toward business policy; business rules remain testable without concrete infrastructure; boundaries express ownership and protect independent evolution. |
| Contextual heuristics | Place behavior with the concept that owns its invariant; prefer cohesive modules and explicit boundaries; introduce ports at meaningful volatility or ownership boundaries; keep composition separate from business decisions. Apply only when project evidence supports them. |
| Opt-in policies | Aggregate transaction conventions; repository style; strict typing; file-size or one-unit-per-file rules; mutation metadata; missing-value semantics; read-model location; immutable containers; shared kernels; compatibility re-exports; framework and language profiles. |

| Situation | Action |
| --- | --- |
| Existing architecture works | Preserve its vocabulary and migrate incrementally. |
| Business boundaries are unclear | Map use cases, invariants, ownership, and consistency before proposing layers. |
| Simple CRUD or low domain complexity | Recommend a simpler structure unless the user explicitly requires hexagonal/DDD. |
| A pattern or abstraction has no observed pressure | Do not introduce it; document the condition that would justify it later. |
| Growth creates navigation or ownership friction | Re-evaluate module boundaries and split by cohesive capability or change pattern, not by a universal directory template. |

## Execution Steps

1. Discover project constraints and establish the requested architecture outcome.
2. Map capabilities, ownership, invariants, dependencies, volatility, integration boundaries, and current composition points using the project's own vocabulary.
3. Identify concrete forces: coupling, low cohesion, unstable dependencies, duplicated policy, framework leakage, navigation cost, or scaling pressure.
4. Classify findings against core invariants, contextual heuristics, and explicitly selected policies. Reject speculative abstractions.
5. Produce one prioritized, deduplicated plan that explains each tradeoff and preserves behavior and required compatibility.
6. Implement or report the smallest coherent change and verify with available tests and architecture checks.

## Output Contract

Return scope and discovered constraints, architectural forces, selected policies or profiles, findings or changes by priority, tradeoffs, verification evidence, and unresolved risks. Distinguish invariant violations from contextual improvements and explicitly state which structures remain intentionally flexible.

## References

- [DEPENDENCY-RULE.md](references/DEPENDENCY-RULE.md) - boundary analysis.
- [DOMAIN-RICHNESS.md](references/DOMAIN-RICHNESS.md) - behavior and invariants.
- [PORT-CONTRACTS.md](references/PORT-CONTRACTS.md) - boundary contracts.
- [COMPOSITION-ROOT.md](references/COMPOSITION-ROOT.md) - dependency wiring.
- [FILE-ORGANIZATION.md](references/FILE-ORGANIZATION.md) - cohesion-based layout.
- [SHARED-KERNEL.md](references/SHARED-KERNEL.md) - cross-context sharing tradeoffs.
- [ANTI-PATTERNS.md](references/ANTI-PATTERNS.md) - diagnostic signals.
- [REFACTORING-RECIPES.md](references/REFACTORING-RECIPES.md) - migration recipes.
- [AUDIT-CHECKLIST.md](references/AUDIT-CHECKLIST.md) - full-audit prompts.
