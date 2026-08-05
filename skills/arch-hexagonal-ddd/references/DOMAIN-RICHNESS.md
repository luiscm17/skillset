# Proportional Domain Modeling

Use `../SKILL.md` as the canonical policy. Model business behavior where its meaning and invariants can be protected, but make richness proportional to actual domain complexity.

## Find the Behavior

Look for decisions that must remain consistent across workflows: valid state transitions, calculations, eligibility, limits, identity, lifecycle rules, or coordinated changes. Place each decision with the concept or policy that owns it, using the project's native constructs.

A model may be too anemic when callers repeatedly interpret state, duplicate the same rule, or mutate related values without one enforcement point. Data-only objects are not inherently anemic when the area is genuinely CRUD, reporting, integration, or transport oriented and has no meaningful behavior to protect.

A model may be artificially rich when it wraps fields in ceremonial types, invents entities without identity or lifecycle, or moves straightforward mapping and workflow code into objects that do not own a business rule.

## Choose the Modeling Unit

| Need | Possible model |
| --- | --- |
| Rule belongs to one concept and its state | Behavior on that concept. |
| Rule combines concepts without natural ownership | Cohesive domain policy or service. |
| Workflow coordinates collaborators and side effects | Application-level orchestration. |
| Data is shaped for transport, querying, or display | Purpose-specific data representation. |
| No relevant invariant exists | Keep the model simple. |

These are choices, not mandatory class categories. Functions, modules, algebraic data types, objects, or other language idioms may express the same responsibilities.

## Contextual Decisions

- Define aggregate boundaries only where a consistency boundary and ownership model require them.
- Introduce repositories for meaningful persistence boundaries, not automatically for every entity or table.
- Choose transaction scope from business consistency, failure behavior, and platform capabilities.
- Treat idempotency, timestamps, version fields, optimistic locking, event publication, and mutation style as explicit project policies.
- Keep domain policy independent of concrete infrastructure, while passing required business facts or capabilities through suitable contracts.

## Review Questions

- What invariant or decision does this model protect?
- Where is the same rule currently duplicated or bypassed?
- Does moving behavior reduce inconsistency, or only relocate code?
- Is orchestration being mistaken for domain behavior?
- Would a simpler data-centric model remain correct and easier to evolve?
- Do consistency and concurrency choices match observed requirements?

Refactor only the smallest behavior slice with clear evidence. Preserve external behavior, add focused tests around the invariant, and avoid redesigning unrelated CRUD paths.
