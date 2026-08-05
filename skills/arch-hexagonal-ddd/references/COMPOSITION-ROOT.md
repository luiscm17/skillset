# Composition Responsibility

Use `../SKILL.md` as the canonical policy. Composition is the responsibility of selecting concrete implementations, wiring dependencies, applying configuration, and managing lifecycle at the system edge. It keeps those decisions out of business policy; it does not require a directory, container framework, application factory, or one central file.

## What Composition Owns

- selecting implementations for the current runtime or deployment;
- constructing dependency graphs and connecting entry points;
- configuration validation and environment-specific choices;
- resource creation, sharing, scoping, startup, shutdown, and cleanup;
- transaction, request, job, or message lifecycle integration where applicable;
- technical error translation and framework registration at an appropriate edge.

Composition may be centralized or distributed across ecosystem-owned entry points. Keep each composition point visible and free of business decisions. Avoid letting runtime service locators or global state hide dependencies from consumers.

## Choose Structure From Pressure

| Evidence | Response |
| --- | --- |
| Wiring is small and stable | Keep it direct. |
| Distinct runtimes or capabilities have independent graphs | Use separate cohesive composition points. |
| Resource lifetimes differ | Make scopes and ownership explicit. |
| Construction repeats and drifts | Extract the smallest reusable composition helper. |
| A dependency-injection tool reduces lifecycle or graph complexity | Use it at the edge without leaking it into business policy. |
| Factories or containers only forward constructors | Remove the ceremony. |

Typed containers, immutable holders, generated wiring, factories, and dependency-injection libraries are optional implementation techniques. Adopt them only when they improve static guarantees, navigation, lifecycle control, or replacement cost.

## Verification

After a contract or constructor changes, trace affected consumers and composition points. Verify that required dependencies are supplied, lifetimes match usage, resources are released, configuration failures are visible, and representative entry points still execute. Scope testing to the change and expand it when shared wiring or lifecycle behavior creates broader risk.

Review composition as code that evolves: split or merge it according to cohesion, ownership, runtime boundaries, and change pressure rather than file size or bounded-context count.
