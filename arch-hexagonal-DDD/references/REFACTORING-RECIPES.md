# Refactoring Recipes

## Recipe 1: Moving a Type to Shared Kernel

**When:** A type is imported by 2+ bounded contexts directly.

1. Create `shared/{module}.py` with the type definition
2. Update original file to re-export from shared (backward compat)
3. Update all direct consumers to import from shared
4. Document the new shared package in project docs

**Pitfall:** Check ALL consumers including test files and nested bootstrap factories.

## Recipe 2: Splitting a Fat File

**When:** A file has 200+ lines with multiple unrelated classes.

1. Create individual files with extracted classes (keep same imports)
2. Move each class to its file, adjusting only its specific imports
3. Replace original file content with re-exports only
4. Update `__init__.py` to import from new locations
5. Run tests — all import paths still resolve via re-exports
6. New code imports from specific modules; old code works unchanged

**Pitfall:** Don't forget to move `@staticmethod` helpers that belong to the moved class.

## Recipe 3: Extracting a Separate Repository

**When:** An entity's persistence methods are on the wrong aggregate's repository.

1. Define the port protocol in `ports/{entity}.py`
2. Create the adapter in `adapters/persistence/{entity}_repository.py`
3. Add re-export to `ports/__init__.py`
4. **GREP ALL CONSUMERS** — every use case, every factory, every test
5. Add the new repository parameter to each consumer's `__init__`
6. Update ALL composition root factories (including nested ones!)
7. Remove methods from the original repository protocol
8. Remove methods from the original repository adapter
9. Run tests

**Critical pitfall:** Nested factories are easy to miss. If your auth factory builds access use cases, it needs the new repository too. Type checker errors will reveal missed consumers — run it before trusting "done."

## Recipe 4: Eliminating `# type: ignore`

| Pattern | Diagnosis | Fix |
| ------- | --------- | --- |
| `_env_file=path  # type: ignore[call-arg]` | Framework kwarg not in stubs | `env_kwargs: dict[str, Any] = {}; if path: env_kwargs["_env_file"] = path; super().__init__(**env_kwargs)` |
| `response.data[0]  # type: ignore[assignment]` | Nullable not narrowed after check | Assign to local: `data = response.data; if not data: return None; row = data[0]` |
| `.value  # type: ignore[union-attr]` | Optional attribute access | Assert non-None: `assert bale.delivery_date is not None` |
| `result.rowcount > 0  # type: ignore[union-attr]` | Generic result type | Annotate: `result: CursorResult[Any] = session.execute(stmt)` |
| `row: dict = data[0]  # type: ignore[assignment]` | Untyped third-party return | `cast(dict[str, Any], data[0])` |

**Principle:** Every type suppression is an architecture smell. Fix the contract.

## Recipe 5: Enriching an Anemic Entity

**When:** Application services directly mutate entity fields.

1. Identify all field mutations in application services (`entity.field = value`)
2. Group related mutations into business actions (deactivate, revoke, grant)
3. Create the behavior method on the entity with:
   - Idempotency guard (if appropriate)
   - Version bump
   - Timestamp update (via `at` parameter)
   - Domain error on invariant violation
4. Create the domain error class if needed
5. Replace inline mutations in use cases with method calls
6. Add unit tests for the new behavior (including idempotency and guards)
7. Run full test suite

**Pitfall:** Test factories may construct entities with positional args. Keep `__init__` signature compatible (add methods, don't change construction).

## Recipe 6: Converting Dict Provider to Typed Container

**When:** A composition factory returns `dict[str, Any]` with use cases.

1. Create a frozen dataclass in `application/containers.py`
2. Add all dict keys as typed attributes
3. Update the factory return type and construct the dataclass
4. Update all consumers: `use_cases["key"]` → `use_cases.key`
5. Update helper functions that received `dict` to receive the container type
6. Remove unused individual imports from consumers (the container carries them)
7. Run tests
