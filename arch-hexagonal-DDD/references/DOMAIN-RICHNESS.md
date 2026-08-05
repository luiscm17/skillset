# Domain Richness

## The Rule

Entities MUST encapsulate their behavior and enforce their own invariants. Application services orchestrate — they do not decide or mutate.

## Anemic vs Rich Pattern

### Anemic (WRONG)

```python
# Application service owns the logic
def deactivate_user(self, user_id: str) -> None:
    user = self._users.find_by_id(user_id)
    if not user.is_active:
        return  # caller knows business rule
    user.is_active = False
    user.version += 1
    user.updated_at = self._clock.now()
    self._users.save(user)
```

### Rich (CORRECT)

```python
# Entity owns the logic
class AccessUser:
    def deactivate(self, *, at: datetime) -> None:
        """Deactivate the user. Idempotent when already inactive."""
        if not self.is_active:
            return
        self.is_active = False
        self.version += 1
        self.updated_at = at

# Application service orchestrates
def deactivate_user(self, user_id: str) -> None:
    user = self._users.find_by_id(user_id)
    user.deactivate(at=self._clock.now())
    self._users.save(user)
```

## Behavior Method Requirements

Every entity mutation method must:

1. **Be named after the business action** — `deactivate()`, `revoke()`, `grant_permission()`
2. **Handle idempotency** — where business semantics allow (deactivate inactive = no-op)
3. **Bump version** — on every state change (optimistic concurrency)
4. **Update timestamps internally** — caller passes `at` parameter, entity sets its own `updated_at`
5. **Raise domain error on invariant violation** — revoke already-revoked = `AssignmentAlreadyRevoked`
6. **Accept only business parameters** — never framework objects, sessions, or repos

## Invariant Guard Pattern

```python
class Assignment:
    def revoke(self, *, by: str, reason: str, at: datetime) -> None:
        """Revoke this assignment. Raises if already revoked."""
        if not self.is_current:
            raise AssignmentAlreadyRevoked()
        self.revoked_by_user_id = by
        self.revoke_reason = reason
        self.revoked_at = at
```

## Duplicate Guard Pattern

```python
class Role:
    def grant_permission(self, permission: Permission) -> None:
        """Add a permission. Raises if already present."""
        if permission in self.permissions:
            raise DuplicateRolePermission()
        self.permissions.add(permission)
```

## Detection: Is My Model Anemic?

Your domain model is anemic if:

- [ ] Application services contain `entity.field = value` patterns
- [ ] Business rules live in services (not in entities)
- [ ] Entities have only data fields and properties
- [ ] Version bumps happen outside the entity
- [ ] Timestamp updates happen outside the entity
- [ ] Callers check state before mutating (instead of entity guarding)

## Application Service After Enrichment

Use cases should read like a script:

```python
def execute(self, command: DeactivateCommand) -> None:
    with self._transaction.atomic():
        user = self._users.find_by_id(command.user_id)
        if user is None:
            raise UserNotFound()
        user.deactivate(at=self._clock.now())
        self._users.save(user)
        self._audits.append(...)
```

Load → call ONE behavior method → save → side effects. No branching on entity state.
