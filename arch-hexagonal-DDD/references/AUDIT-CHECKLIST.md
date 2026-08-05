# Architecture Audit Checklist

Run this after every structural change. Each section maps to a layer.

## Layer 1: Domain

- [ ] Entities have behavior methods (not just data fields)
- [ ] Behavior methods are idempotent where business semantics allow
- [ ] Invariants are enforced inside entities (not in application services)
- [ ] No framework imports (stdlib only)
- [ ] No Optional where state is guaranteed post-construction
- [ ] Domain errors are specific and named after the business rule violated
- [ ] Value objects are immutable (frozen dataclass)
- [ ] No technology/library names in class or module names

## Layer 2: Ports

- [ ] Return types are fully specified (no bare `list`, no `Any`)
- [ ] One file per protocol
- [ ] No framework types (no Session, no Response, no Engine)
- [ ] Ports describe WHAT (business intent), never HOW (implementation)
- [ ] Protocol methods use domain types exclusively
- [ ] No `Optional` leaking where the domain guarantees a value

## Layer 3: Application

- [ ] Use cases call entity behavior (not inline field mutation)
- [ ] One file per use case
- [ ] No adapter imports (only ports and domain)
- [ ] Transaction boundary is at the use case level
- [ ] Use case reads like a script: load → call behavior → save → side effects
- [ ] DTOs are plain dataclasses (no framework decorators)

## Layer 4: Adapters

- [ ] One file per adapter class
- [ ] No `# type: ignore` in any adapter
- [ ] Each adapter implements exactly one port protocol
- [ ] Framework-specific types stay inside adapters (never leak to ports)
- [ ] ORM records are separate from domain entities
- [ ] Mapping between domain and persistence is explicit

## Layer 5: Composition Root

- [ ] Typed containers for use case providers (no `dict[str, Any]`)
- [ ] All consumers wired (grep for old method calls after any port change)
- [ ] No orphan code (every use case wired, every port has an adapter)
- [ ] One factory file per bounded context
- [ ] Shared infra (Clock, Identity) not duplicated across factories

## Cross-Cutting Verification

- [ ] Zero dependency rule violations
- [ ] Zero `# type: ignore` in `src/` (tests exempt)
- [ ] No technology names in domain or port layers
- [ ] No cross-context imports (use shared kernel or ACL adapter)
- [ ] All tests pass (unit + integration)
- [ ] No orphan references (grep for removed method names)
- [ ] Re-export files exist after every file split (backward compat)

## Automated Verification Commands

```bash
# Zero type suppressions
grep -r "type: ignore" src/

# Dependency rule (requires AST script from DEPENDENCY-RULE.md)
python check_dependencies.py

# Orphan method references
grep -rn "old_method_name" src/

# All tests
python -m unittest discover -s tests -v
```
