# Dependency Rule

Dependencies point **inward only**. Outer layers depend on inner layers, never the reverse.

## Layer Hierarchy

```text
bootstrap (composition root) → knows everything
adapters (infrastructure)    → knows application + domain + ports
application (use cases)      → knows domain + ports
ports (contracts)            → knows domain only
domain (core)                → knows NOTHING external
```

## Allowed and Forbidden Imports

| Layer | May Import From | Must NOT Import From |
| ----- | --------------- | -------------------- |
| domain | stdlib only | application, ports, adapters, bootstrap, infra, ANY framework |
| ports | domain | application, adapters, bootstrap, infra, ANY framework |
| application | domain, ports | adapters, bootstrap, ANY framework |
| adapters | domain, ports, application, frameworks | bootstrap (circular) |
| bootstrap | everything | (it's the root) |

## Verification Script (Python Example)

```python
import ast
from pathlib import Path

LAYER_RULES = {
    "domain": ["application", "adapters", "ports", "bootstrap", "infra", "fastapi", "sqlalchemy", "pydantic"],
    "ports": ["application", "adapters", "bootstrap", "infra", "fastapi", "sqlalchemy"],
    "application": ["adapters", "bootstrap", "fastapi", "sqlalchemy"],
}

def check_violations(src: Path, context: str) -> list[str]:
    violations = []
    for f in src.rglob("*.py"):
        parts = str(f.relative_to(src)).split("/")
        layer = next((p for p in parts if p in LAYER_RULES), None)
        if not layer:
            continue
        tree = ast.parse(f.read_text())
        for node in ast.walk(tree):
            if isinstance(node, ast.ImportFrom) and node.module:
                for forbidden in LAYER_RULES[layer]:
                    if node.module.startswith(forbidden):
                        violations.append(f"{f}:{node.lineno} → {node.module}")
    return violations
```

## Common Violations and Fixes

| Violation | Example | Fix |
| --------- | ------- | --- |
| Domain imports ORM | `from sqlalchemy import Column` in entity | Use plain dataclass; map in adapter |
| Port imports framework type | `Session` in protocol signature | Use abstract types; inject session in adapter |
| Application imports adapter | `from adapters.persistence import Repo` | Import the PORT protocol instead |
| Domain imports port | `from ports.users import UserRepo` in entity | Entity receives collaborators via method args, not imports |

## The Litmus Test

> "Create your application to work without either a UI or a database."
> — Alistair Cockburn

If you can instantiate your domain objects and run business logic from a plain unit test with zero infrastructure, your boundaries are correct.
