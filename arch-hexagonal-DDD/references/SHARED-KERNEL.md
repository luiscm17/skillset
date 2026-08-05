# Shared Kernel

## When to Use

A type belongs in a shared kernel when:

- It is consumed by 2+ bounded contexts
- It represents a provider-neutral concept (not tied to one context)
- It is a value object (immutable, no lifecycle)
- Examples: `AuthenticatedIdentity`, `IdentityResolver`, `Money`, `Email`

A type does NOT belong in shared kernel if:

- It is used by only one context (keep it in that context)
- It has mutable state or lifecycle (it's an entity, not a shared value)
- It contains business rules specific to one context

## Package Structure

```text
src/
├── shared/              # Shared kernel package
│   ├── __init__.py      # Package marker
│   └── identity.py      # AuthenticatedIdentity, IdentityResolver
├── access/              # Bounded context (consumes shared)
├── warehouse/           # Bounded context (consumes shared)
└── auth/                # Bounded context (consumes shared)
```

**Key:** `shared/` is NOT a bounded context. It is a kernel — a small set of types that all contexts agree on. It has no behavior, no use cases, no adapters.

## Migration Pattern: Moving a Type to Shared

### Step 1: Create the shared module

```python
# shared/identity.py
from dataclasses import dataclass
from collections.abc import Callable

@dataclass(frozen=True, slots=True)
class AuthenticatedIdentity:
    subject: str
    session_id: str | None = None

IdentityResolver = Callable[..., AuthenticatedIdentity]
```

### Step 2: Re-export from the original location

```python
# warehouse/bales/ports/authorization.py (original home)
from shared.identity import AuthenticatedIdentity as AuthenticatedIdentity
from shared.identity import IdentityResolver as IdentityResolver

# Keep context-specific types here
class AuthorizationDenied(Exception): ...
class AuthorizationPort(Protocol): ...
```

### Step 3: Update direct consumers

All imports in your current scope should now point to `shared.identity` directly. The re-export keeps external consumers working.

### Step 4: Document in AGENTS.md

Add `shared` to the recognized top-level packages list.

## Cross-Context Communication Without Shared Kernel

When contexts need to communicate but don't share types:

1. **ACL Adapter** — context A provides an adapter that translates its types for context B's port
2. **Integration Events** — publish domain events; subscribers map to their own types
3. **Separate DTOs** — each context defines its own DTO; a mapper bridges them

The shared kernel is for AGREED-UPON types. If contexts disagree on the shape, use ACL instead.
