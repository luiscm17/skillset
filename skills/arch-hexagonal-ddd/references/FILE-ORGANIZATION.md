# File Organization

## Core Principle: One File Per Concern

Every layer follows the same rule: one file per meaningful unit.

| Layer | Unit | Naming Convention | Example |
| ----- | ---- | ----------------- | ------- |
| `domain/` | One file per aggregate/concept | `{aggregate}.py` | `users.py`, `roles.py`, `scopes.py` |
| `ports/` | One file per port protocol | `{aggregate}.py` | `users.py`, `roles.py`, `audit.py` |
| `application/` | One file per use case | `{verb}_{noun}.py` | `create_role.py`, `deactivate_user.py` |
| `adapters/persistence/` | One file per adapter class | `{aggregate}_repository.py` | `user_repository.py`, `role_repository.py` |
| `adapters/http/` | Grouped by router scope | descriptive | `router.py`, `models.py`, `error_handlers.py` |
| `bootstrap/` | One file per context factory | `{context}_dependency.py` | `access_admin_dependency.py` |

## When to Split

A file needs splitting when:

- It exceeds ~200 lines AND contains multiple classes/protocols
- It contains unrelated concerns (e.g., user + role + audit adapters)
- Multiple developers frequently edit different sections of the same file

A file does NOT need splitting just because it's long if it has a single cohesive responsibility.

## Target Directory Structure

```text
src/
├── shared/                         # Shared kernel (NOT a bounded context)
│   └── identity.py
├── {context}/                      # One bounded context
│   ├── domain/
│   │   ├── {aggregate}.py          # Entity + value objects for that aggregate
│   │   ├── errors.py               # Domain errors
│   │   └── audit.py                # Read models (if needed)
│   ├── ports/
│   │   ├── __init__.py             # Re-exports all ports
│   │   ├── users.py                # One protocol per file
│   │   ├── roles.py
│   │   ├── assignments.py
│   │   ├── clock.py
│   │   └── transaction.py
│   ├── application/
│   │   ├── __init__.py
│   │   ├── create_role.py          # One use case per file
│   │   ├── deactivate_user.py
│   │   ├── containers.py           # Typed use case container
│   │   └── dto.py                  # Shared command/result DTOs
│   └── adapters/
│       ├── persistence/
│       │   ├── user_repository.py  # One adapter per file
│       │   ├── role_repository.py
│       │   ├── records.py          # ORM records/table models
│       │   └── transaction.py
│       ├── http/
│       │   ├── router.py
│       │   ├── models.py           # Request/Response schemas
│       │   └── error_handlers.py
│       └── {acl_adapter}.py        # Anti-corruption layer adapters
└── bootstrap/
    ├── http_application.py         # Composition root
    ├── {context}_dependency.py     # Per-context factory
    ├── api_router.py               # Route composition
    └── database_session_dependency.py
```

## Backward Compatibility After Split

When splitting a fat file:

1. Create the individual files with the extracted classes
2. Replace the original file content with re-exports only
3. Existing consumers continue working (import path unchanged)
4. New code imports from the specific module directly

```python
# Original file (now a re-export shim)
"""Backward-compatible re-exports from split modules."""
from module.ports.users import UserRepository as UserRepository
from module.ports.roles import RoleRepository as RoleRepository
```

## Package `__init__.py` Convention

Each layer's `__init__.py` re-exports all public types for convenience:

```python
"""Access Control ports."""
from access.ports.users import AccessUserRepository as AccessUserRepository
from access.ports.roles import RoleRepository as RoleRepository
from access.ports.assignments import AssignmentRepository as AssignmentRepository
```

This allows both:

- `from access.ports import AccessUserRepository` (convenient)
- `from access.ports.users import AccessUserRepository` (explicit)
