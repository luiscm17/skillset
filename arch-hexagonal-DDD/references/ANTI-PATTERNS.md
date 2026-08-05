# Anti-Pattern Detection

## Anemic Domain Model

**Signals:**

- Application services with `entity.field = value` patterns
- Business rules implemented in services, not entities
- Entities with only getters/properties and no mutation methods
- Version bumps done by the caller, not the entity

**Fix:** Move behavior INTO entities. See DOMAIN-RICHNESS.md.

## Leaking Infrastructure

**Signals:**

- Domain error classes referencing HTTP status codes
- Port protocols using framework-specific types
- Application services importing from adapter packages
- Domain entities with ORM decorators/base classes
- Technology names in domain/port layer modules

**Fix:** Extract framework concerns to adapters. Ports use only domain types.

## Namespace Violations

**Signals:**

- Importing from another bounded context's internal modules
- Technology/library names in domain or port class names
- Adapter classes used directly in application services (bypassing port)

**Fix:** Use shared kernel for cross-context types. Use ACL adapters for
cross-context communication. Rename to business vocabulary.

## Orphan Code After Refactoring

**Signals:**

- Type checker errors on removed methods (e.g., `save_assignment` on a repo that no longer has it)
- Import paths that no longer resolve
- Dict key access on a refactored typed container
- Methods on a protocol that no adapter implements
- Wiring in bootstrap that constructs use cases without new required parameters

**Fix:** After every port/repository change, grep ALL consumers including
nested factories. Type checker errors are your friend — they reveal missed consumers.

## Fat Files

**Signals:**

- Multiple unrelated protocols in one ports file (>200 lines)
- Multiple adapter classes in one file (>200 lines)
- A composition root function exceeding 100 lines

**Fix:** Split into one-file-per-concern. See FILE-ORGANIZATION.md and REFACTORING-RECIPES.md.

## Repository Per Entity

**Signals:**

- Multiple repositories for entities within the same aggregate
- Repository methods for child entities on the aggregate root's repository

**Fix:** One repository per AGGREGATE. If an entity has independent lifecycle,
it's its own aggregate — give it its own repository and port.

## Stringly-Typed Providers

**Signals:**

- `dict[str, Any]` as use case container
- `use_cases["create_role"]` string-key access
- No compile-time verification of container contents

**Fix:** Frozen dataclass container. See COMPOSITION-ROOT.md.

## Cross-Aggregate Transaction

**Signals:**

- Multiple aggregates loaded and saved in one transaction
- Use case modifying entities from different aggregate roots atomically

**Fix:** One aggregate per transaction. Use domain events for eventual
consistency across aggregates. Only exception: if business absolutely
requires atomicity and the cost of eventual consistency is unacceptable.
