# Capability-Oriented Organization

Organize the system so product capabilities, ownership, and public boundaries are discoverable before technical mechanisms. `../SKILL.md` is canonical; examples here illustrate the style and never mandate names or directories.

## Architectural Properties

- Code that changes for one product responsibility stays near its owner.
- Each capability can evolve behind a narrow public contract.
- Internal responsibility zones are visible only when they improve cohesion or navigation.
- Application composition is distinguishable from capability-owned decisions.
- Shared code has explicit ownership and a smaller surface than capability code.

## Illustrative Shapes

A compact capability may remain flat:

```text
capabilities/
  purchasing/
    public
    policy
    view
    data-source
composition/
```

A capability under greater change pressure may expose conceptual zones:

```text
capabilities/
  purchasing/
    public-contract/
    policy-and-orchestration/
    interface/
    state-and-adapters/
composition/
shared-technical/
```

The labels are placeholders. A language may represent the same boundaries with files, modules, packages, namespaces, or export controls. Do not create empty zones or mirror this tree mechanically.

## Flatten, Split, or Adapt

| Evidence | Response |
| --- | --- |
| Responsibilities evolve together and navigation is clear | Flatten; a public surface and a few cohesive units may be enough. |
| Policy, presentation, state, or integration changes independently | Split only the responsibility with demonstrated pressure. |
| A capability has unrelated product owners or lifecycles | Reassess the capability boundary before adding technical layers. |
| The proposed boundary only forwards calls | Merge or remove it. |
| Ecosystem entry points require a particular location | Keep the edge conventional while routing decisions to the capability owner. |
| Shared code accumulates product decisions | Return each decision to its capability or establish genuine universal ownership. |

Use the weakest physical boundary that protects the needed independence. Strengthen it only for demonstrated ownership, release, scale, security, lifecycle, or navigation pressure. Never use file size or visual symmetry as standalone evidence.

## Review Questions

- Can a maintainer find a product capability without first tracing technical types?
- Does every physical boundary protect a real responsibility or owner?
- Could a flatter organization preserve the same invariants?
- Are public and internal surfaces distinguishable by an enforceable mechanism?
- Can zones split, merge, or be renamed without changing the architectural model?
