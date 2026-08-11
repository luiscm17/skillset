---
name: frontend-arch
description: "Trigger: frontend architecture, capability boundaries, public contracts, composition, shared code. Apply a capability-oriented, contract-driven style."
license: Apache-2.0
metadata:
  author: luiscm17
  version: "2.0.0"
---

# Capability-Oriented, Contract-Driven Frontend Architecture

## Activation Contract

Use for frontend architecture design, organization, audit, migration, boundary changes, public API design, composition, or shared extraction. Apply **Capability-Oriented, Contract-Driven Frontend Architecture** as an architectural style, not a folder template. Do not activate for isolated visual work, routine cleanup, or local defects without architectural pressure.

## Hard Rules

- Organize first by product capability ownership, never by global technical type.
- Give each capability a narrow public contract. Consumers MUST NOT import its internals; cross-capability collaboration uses stable public entry points and explicit contracts.
- Let application composition depend on capabilities; capabilities MUST NOT depend on the composition shell.
- Keep shared code intentionally small, technical or genuinely universal, and explicitly owned. Visual similarity alone never justifies extraction.
- Assign one canonical owner and source to every responsibility and value. Derive state instead of synchronizing duplicates.
- Separate responsibility zones inside a capability only as evidence requires: public contract, policy or orchestration, presentation, state, adapters, and integration are concepts, not mandatory layers or directories.
- Require observed ownership, coupling, lifecycle, or change pressure for abstractions and stronger boundaries. Preserve bounded, one-directional migrations with explicit retirement proof.
- Preserve ecosystem idioms and technology independence. Use one architecture worker by default; delegate only independent uncertainty at depth one, then synthesize one decision.
- Treat the gates below as canonical. Load at most two references by default; a full audit may load all references sequentially.

## Decision Gates

| Level | Policies |
| --- | --- |
| Core invariants | Capability-first ownership; contract-only boundary crossings; shell-to-capability dependency direction; singular authority; deliberately small shared surface. |
| Contextual heuristics | Separate zones when responsibilities change independently; flatten cohesive code; split on proven ownership, lifecycle, scale, or navigation pressure; introduce contracts at meaningful seams. |
| Opt-in organization policies | Named zone directories; one public entry file; import enforcement; strict contract typing; compatibility exports; dedicated integration modules; maximum shared surface. |

| Situation | Action |
| --- | --- |
| Capability remains cohesive | Keep it flat or extend its canonical owner. |
| Responsibilities change independently | Split only the affected conceptual zones. |
| Another capability needs behavior | Expose the smallest semantic public contract; never reveal internals. |
| Reuse has only visual or syntactic similarity | Keep it local. |
| Migration lacks one direction or retirement proof | Reject the parallel authority. |

## Execution Steps

1. Discover capabilities, owners, consumers, state sources, contracts, composition points, constraints, and available verification.
2. Map dependency crossings and change pressure; classify evidence against core invariants, contextual heuristics, and selected opt-in policies.
3. Choose the smallest organization and contract change that restores ownership, direction, cohesion, and singular authority.
4. Define compatibility bounds, migration direction, retirement condition, and focused verification before implementation.
5. Implement or report one deduplicated change set; verify public surfaces, forbidden internal dependencies, composition direction, state authority, and retired paths.

## Output Contract

Return activation fit; scope and evidence; capability and ownership map; invariant violations versus contextual improvements; selected opt-in policies; findings or changes by priority; illustrative organization only when useful; contract and dependency effects; migration and retirement conditions; verification evidence; intentionally flexible structures; unresolved risks.

## References

- [ORGANIZATION.md](references/ORGANIZATION.md) - adaptable capability organization examples.
- [DEPENDENCY-RULES.md](references/DEPENDENCY-RULES.md) - boundary and dependency direction.
- [PUBLIC-CONTRACTS.md](references/PUBLIC-CONTRACTS.md) - capability public API guidance.
- [COMPOSITION.md](references/COMPOSITION.md) - application wiring and coordination.
- [SHARED-AND-REUSE.md](references/SHARED-AND-REUSE.md) - shared ownership and extraction.
- [ANTI-PATTERNS.md](references/ANTI-PATTERNS.md) - evidence-driven diagnostic signals.
- [MIGRATION-RECIPES.md](references/MIGRATION-RECIPES.md) - bounded migration recipes.
- [AUDIT-CHECKLIST.md](references/AUDIT-CHECKLIST.md) - full architecture audit.
