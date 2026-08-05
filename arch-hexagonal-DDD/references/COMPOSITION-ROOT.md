# Composition Root

## Role

The composition root (`bootstrap/`) is the ONLY place that:

- Knows all layers exist
- Imports from adapters, application, domain, and ports
- Creates concrete instances and wires dependencies
- Is NOT imported by any other layer

## File Organization

| File | Responsibility |
| ---- | -------------- |
| `http_application.py` | App creation, settings, middleware, identity resolver |
| `{context}_dependency.py` | Per-context factory building use cases from adapters |
| `api_router.py` | Top-level route composition (includes all context routers) |
| `database_session_dependency.py` | Session factory and lifecycle |
| `http_error_handlers.py` | Exception → HTTP response mapping |

## Typed Containers (Mandatory)

Never pass use cases as `dict[str, Any]`. Create a frozen dataclass:

```python
@dataclass(frozen=True, slots=True)
class AdminUseCases:
    list_users: ListUsers
    list_roles: ListRoles
    create_role: CreateRole
    replace_user_roles: ReplaceUserRoles
    user_repository: AccessUserRepository  # for actor resolution
    identity: IdentityPort                 # for operation ID generation
```

**Benefits:**

- Compile-time type checking (no `use_cases["typo"]` bugs)
- IDE autocompletion
- Explicit dependency documentation
- Frozen = immutable after construction

## Factory Pattern

Each context dependency file follows this pattern:

```python
def admin_use_case_dependency(
    session_provider: SessionProvider,
) -> Callable[..., AdminUseCases]:
    """Build request-scoped admin use cases."""
    
    clock = SimpleClock()
    identity = SimpleIdentity()

    def provide(session: Annotated[Session, Depends(session_provider)]) -> AdminUseCases:
        user_repo = UserRepositoryAdapter(session)
        role_repo = RoleRepositoryAdapter(session)
        # ... wire all adapters and use cases
        return AdminUseCases(
            list_users=ListUsers(user_repository=user_repo),
            # ...
        )

    return provide
```

## When to Split

Extract a `_compose_X` function into its own `{context}_dependency.py` when:

- The function exceeds ~100 lines
- A new bounded context needs its own wiring
- The function returns multiple unrelated values
- You see duplicate Clock/Identity instances across factories

## Shared Infrastructure

Clock and Identity generators that appear in multiple factories should be extracted to a shared location (e.g., `infra/clock.py`, `infra/identity.py`) rather than duplicated as private classes in each factory.

## Wiring Completeness Check

After ANY port/repository change:

1. Grep for the old method name across ALL of `bootstrap/`
2. Check nested factories (e.g., auth factory that builds access use cases)
3. Verify the typed container includes new parameters
4. Run the full test suite — missing wiring = runtime `TypeError`
