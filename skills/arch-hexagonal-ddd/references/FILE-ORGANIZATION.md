# Evolutionary Code Organization

Organize code to expose business capabilities, ownership, and dependency boundaries. Do not infer architecture quality from directory names, file counts, or a universal project tree. The canonical policies in `../SKILL.md` take precedence.

## Architectural Properties

Prefer an organization where:

- related behavior and data change together;
- business capabilities are discoverable without tracing framework internals;
- dependencies cross explicit boundaries for a concrete reason;
- infrastructure can change without rewriting business policy;
- independently evolving areas can be tested and maintained independently;
- navigation cost remains proportional as the codebase and team grow.

These properties may be implemented with packages, modules, namespaces, components, crates, assemblies, folders, or language-specific constructs. Names such as `domain`, `ports`, `adapters`, `application`, and `infrastructure` are optional vocabulary, not architectural requirements.

## Choosing Module Boundaries

Use observed forces rather than fixed templates:

| Signal | Consider |
| --- | --- |
| Code changes for the same business reason | Keep it within one cohesive module or capability. |
| A module changes for unrelated reasons | Split along responsibility, ownership, or capability boundaries. |
| Multiple areas require different release or scaling behavior | Introduce a stronger module or deployment boundary if the operational benefit justifies it. |
| Framework details dominate navigation | Move business-facing entry points and policy into discoverable modules. |
| Cross-module imports grow without clear direction | Clarify ownership, expose a deliberate contract, or merge an artificial boundary. |
| A boundary adds forwarding code but no isolation | Remove or simplify it. |
| Parallel teams repeatedly conflict in the same area | Revisit ownership and capability boundaries before adding layers. |

## Splitting and Merging

Split a file or module when it contains independently changing responsibilities, hides distinct business concepts, creates ownership conflicts, or makes focused testing and navigation materially harder.

Keep it together when its parts enforce one invariant, evolve together, and separating them would create indirection without independent value.

Merge modules when their boundary is accidental, their contracts merely mirror each other, or they cannot evolve independently. Architecture includes removing boundaries that no longer earn their cost.

Do not use line count, number of classes, or one-unit-per-file as standalone decisions. They are local style policies only when the project explicitly adopts them.

## Scaling the Organization

Prefer the smallest boundary that protects the required independence:

1. cohesive file or language unit;
2. module or package with an intentional public surface;
3. business-capability or bounded-context boundary;
4. independently deployable component only when operational independence is required.

Do not jump to a stronger boundary solely because the project is expected to grow. Strengthen boundaries when coupling, ownership, release cadence, reliability, data ownership, or scaling requirements provide evidence.

When growth changes those forces, reorganize incrementally. Preserve compatibility only where consumers require it; do not retain obsolete namespaces or re-export layers by default.

## Language and Framework Adaptation

Follow the ecosystem's conventional discovery and packaging mechanisms unless they violate an architectural invariant. A language may favor feature modules, packages, namespaces, assemblies, crates, or flat files; the skill must adapt rather than translate every project into a Python-style tree.

Framework-owned entry points may remain where the framework expects them. Keep business decisions behind those entry points without inventing duplicate wrappers that add no isolation.

## Review Questions

- Can a maintainer find a business capability without knowing the framework first?
- Does each boundary protect ownership, an invariant, volatility, or independent evolution?
- Are dependencies deliberate and directionally clear?
- Can modules be split or merged without changing unrelated behavior?
- Is an abstraction solving current pressure or only anticipating hypothetical growth?
- Would a simpler organization preserve the same architectural properties?

Report tradeoffs and evidence. Never prescribe a full directory tree unless the user explicitly asks for an ecosystem-specific example.
