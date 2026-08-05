# Port Contracts

## The Rule

Ports define EXACT types that cross boundaries. They make invalid states unrepresentable at the type level.

## Precision Requirements

| Requirement | Bad | Good |
| ----------- | --- | ---- |
| Typed returns | `def list_recent() -> list` | `def list_recent() -> list[AuditEntry]` |
| No false Optional | `name: str | None` (always set) | `name: str` |
| No dict returns | `-> dict` | `-> AuditEntry` (frozen dataclass) |
| No Any | `-> Any` | `-> CursorResult[Any]` (narrowed) |
| No type suppression | `# type: ignore` | Fix the contract |

## Read Models in Ports

When a port needs to return structured data from a query (not an entity), define a frozen read model in the domain layer:

```python
# domain/audit.py
@dataclass(frozen=True, slots=True)
class AccessAuditEntry:
    audit_id: str
    operation_id: str
    change_kind: str
    subject_type: str
    subject_id: str
    performed_by_user_id: str | None
    reason: str | None
    occurred_at: datetime
```

The port then returns this typed read model:

```python
# ports/audit.py
class AccessAuditRepository(Protocol):
    def list_recent(self, *, limit: int = 50) -> list[AccessAuditEntry]: ...
```

## Protocol Definition Pattern

```python
from typing import Protocol

class UserRepository(Protocol):
    """Resolve and persist user state."""

    def find_by_id(self, user_id: str) -> User | None: ...
    def find_by_subject(self, subject: str) -> User | None: ...
    def save(self, user: User) -> None: ...
    def list_all(self) -> list[User]: ...
```

**Rules for protocols:**

- One protocol per file
- Methods use domain types only (never ORM, never framework)
- `None` return means "not found" (never raise for missing)
- `save()` handles both create and update (adapter decides)
- Docstrings describe business intent, not implementation

## Type Suppression is an Architecture Error

Every `# type: ignore` signals one of:

| Cause | Real Fix |
| ----- | -------- |
| Framework kwarg not in stubs | `**kwargs: dict[str, Any]` spread |
| Nullable after truthy check | Assign to local variable first |
| Union not narrowed | `assert` or guard clause |
| Generic result type | Annotate with specific type |
| Third-party untyped return | `cast()` with known runtime type |

**Policy:** Zero type suppressions in source code (tests are exempt). Fix the contract, not the symptom.
