# Shared Kernel and Context Relationships

A Shared Kernel is an explicit DDD relationship in which two or more bounded contexts jointly own a deliberately small model. It is an opt-in governance and coupling decision, not a default reuse mechanism. The canonical policies in `../SKILL.md` take precedence.

## Start With Context Boundaries

Before sharing a model, establish:

- which bounded contexts are involved;
- what each term means inside each context;
- who owns changes and approves compatibility decisions;
- whether the contexts can evolve and release independently;
- whether translation is cheaper and safer than coordinated ownership.

Do not create a Shared Kernel merely because code looks duplicated, two consumers use similar fields, or a type appears technically generic. Similar structure does not prove shared meaning.

## Decision Guide

| Evidence | Prefer |
| --- | --- |
| Contexts intentionally share meaning, invariants, and lifecycle of a stable concept | Consider a small Shared Kernel. |
| Concepts look similar but carry different business meaning | Separate models with explicit translation. |
| One context owns the model and others consume it | Customer/Supplier or Conformist relationship, depending on influence. |
| A downstream context must protect its model from upstream change | Anti-Corruption Layer. |
| Contexts exchange facts without sharing internal models | Published Language, integration events, or versioned messages. |
| Duplication is small and contexts change independently | Prefer duplication over coupling. |
| Ownership or compatibility policy is unclear | Do not introduce a Shared Kernel yet. |

## What May Be Shared

A Shared Kernel may contain a carefully governed subset of:

- value objects with identical semantics;
- domain concepts or policies genuinely owned by all participating contexts;
- stable contracts required for coordinated behavior;
- supporting types whose changes follow the same approval process.

It is not required to be behavior-free. Shared behavior is valid when its invariant and meaning are truly common. Keep the kernel small because every shared element reduces independent evolution.

Do not place a concept in the kernel when it:

- has context-specific rules or terminology;
- changes at a different cadence for each consumer;
- exists only to avoid writing a mapper;
- exposes persistence, transport, framework, or vendor details;
- becomes a general dumping ground for common utilities.

Technical utilities are shared libraries, not automatically part of the DDD Shared Kernel relationship.

## Ownership and Evolution

Define before adoption:

1. participating contexts and maintainers;
2. semantic scope and explicit exclusions;
3. compatibility and versioning policy;
4. change approval and migration process;
5. tests that protect shared meaning;
6. an exit strategy if the contexts diverge.

Prefer a narrow public surface. A new consumer does not automatically gain ownership or justify expanding the kernel. Reassess the relationship when team boundaries, release cadence, semantics, or operational constraints change.

## Placement Is Project-Specific

The physical location may be a module, package, namespace, assembly, crate, library, or another ecosystem-native unit. Its name and position are local design choices. Do not prescribe `shared/`, `common/`, `src/`, or any bounded-context names.

The important properties are:

- ownership is visible;
- dependencies are intentional;
- the kernel does not depend on context-specific infrastructure;
- consumers cannot silently redefine its semantics;
- the boundary can be extracted, merged, or removed as the system evolves.

## Alternatives to Sharing

- **Anti-Corruption Layer:** translate another context's model into local concepts.
- **Published Language:** exchange a documented, versioned integration representation.
- **Integration events:** communicate facts while each context owns its internal model.
- **Open Host Service:** expose a stable protocol for multiple consumers.
- **Separate models and mappers:** accept duplication to preserve autonomy.
- **Merge contexts:** when the supposed boundary has no independent language, ownership, or evolution.

Choose the relationship that minimizes harmful coupling while preserving the business collaboration required.

## Review Questions

- Do all participants assign exactly the same business meaning to the shared concept?
- Is shared ownership explicit rather than inferred from reuse?
- What independent changes become harder after sharing?
- Could translation or duplication preserve autonomy at lower cost?
- Are compatibility, testing, migration, and exit responsibilities defined?
- Is the kernel still small, cohesive, and free of unrelated conveniences?

Report the context relationship, evidence, ownership cost, alternatives considered, and expected evolution. Never prescribe a package tree or copy project-specific names into another codebase.
