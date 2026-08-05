---
name: arch-hexagonal-ddd
description: Prescriptive Hexagonal Architecture + DDD operational skill. Covers dependency rule enforcement, file organization, domain richness, port/adapter contracts, namespace conventions, shared kernel, composition root, refactoring recipes, and audit checklists. Language-agnostic with concrete rules. Triggers on hexagonal, DDD, ports, adapters, domain model, aggregate, bounded context, composition root, dependency direction, anemic model, type safety, shared kernel, repository protocol, use case orchestration, namespace cleanup, architecture audit.
---

# Hexagonal Architecture + DDD — Operational Skill

Prescriptive architecture rules derived from real refactoring experience. This skill tells you WHAT to check, WHERE violations hide, and HOW to fix them.

## Core Invariants (Non-Negotiable)

1. **Dependency Rule** — dependencies point inward only: adapters → application → domain
2. **No Technology Names** — domain and port layers use business vocabulary exclusively
3. **Rich Domain Model** — entities encapsulate behavior and enforce invariants
4. **Port Contract Precision** — fully typed, no suppressions, no `Any`, no bare collections
5. **One Aggregate = One Repository** — never per-entity, always per-consistency-boundary

## Quick Decision Tree

```text
Where does this code go?
├─ Pure business logic, no I/O           → domain/
├─ Orchestrates domain + has side effects → application/
├─ Talks to external systems              → adapters/
├─ Defines WHAT (interface/protocol)      → ports/
├─ Implements a port                      → adapters/
└─ Wires everything together              → bootstrap/
```

## Reference Documentation

Read the matching file before doing the task in the left column:

| Before you...                                                              | Read                                                                   |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Enforce dependency direction, verify layer boundaries, or audit violations | [references/DEPENDENCY-RULE.md](references/DEPENDENCY-RULE.md)         |
| Organize files, split fat modules, or decide package structure             | [references/FILE-ORGANIZATION.md](references/FILE-ORGANIZATION.md)     |
| Write or review domain entities, behavior methods, or invariant guards     | [references/DOMAIN-RICHNESS.md](references/DOMAIN-RICHNESS.md)         |
| Define or tighten port contracts, eliminate type suppressions              | [references/PORT-CONTRACTS.md](references/PORT-CONTRACTS.md)           |
| Relocate types to shared kernel, handle cross-context dependencies         | [references/SHARED-KERNEL.md](references/SHARED-KERNEL.md)             |
| Wire the composition root, create typed containers, split factories        | [references/COMPOSITION-ROOT.md](references/COMPOSITION-ROOT.md)       |
| Refactor: split repos, extract aggregates, eliminate type:ignore           | [references/REFACTORING-RECIPES.md](references/REFACTORING-RECIPES.md) |
| Run a full architectural audit after changes                               | [references/AUDIT-CHECKLIST.md](references/AUDIT-CHECKLIST.md)         |
| Detect anti-patterns and orphan code                                       | [references/ANTI-PATTERNS.md](references/ANTI-PATTERNS.md)             |

## When to Use (and When NOT to)

| Use When                                   | Skip When                                |
| ------------------------------------------ | ---------------------------------------- |
| Complex business domain with many rules    | Simple CRUD, few business rules          |
| Long-lived system (years of maintenance)   | Prototype, MVP, throwaway code           |
| Multiple bounded contexts                  | Single module, < 500 lines total         |
| Need to swap infrastructure (DB, broker)   | Fixed infrastructure, unlikely to change |
| High type-safety / zero-suppression policy | Quick scripts, internal tools            |

## Sources

- Hexagonal Architecture — Alistair Cockburn (2005)
- Domain-Driven Design — Eric Evans (2003)
- The Clean Architecture — Robert C. Martin (2012)
- Implementing Domain-Driven Design — Vaughn Vernon (2013)
