# Architecture Audit

Use only for an explicitly requested full audit. Inspect all relevant areas sequentially, gather evidence, and produce one prioritized, deduplicated result. Do not delegate or launch work per question. `../SKILL.md` remains canonical.

## Establish Scope

- Record the requested outcome, current architecture vocabulary, ecosystem constraints, compatibility obligations, and available verification.
- Select representative business paths and boundaries; do not assume named layers or a fixed project structure.
- Record which contextual heuristics or opt-in policies the project has actually chosen.

## Evidence Questions

### Business Policy

- Can important business rules run without concrete infrastructure?
- Are real invariants enforced consistently at a clear owner?
- Is domain richness proportional, including deliberately simple data-centric areas?
- Do consistency, transaction, time, identity, and concurrency decisions match requirements?

### Boundaries and Dependencies

- What ownership, volatility, or independent-evolution need does each significant boundary protect?
- Do dependencies carry concrete infrastructure into business policy?
- Are cycles, shared data, runtime lookup, or deployment coupling bypassing intended direction?
- Are ports cohesive and justified, with clear success, absence, error, and interaction semantics?
- Are shared models governed explicitly, or would translation or duplication preserve autonomy better?

### SOLID and Patterns

- Does each abstraction solve observed coupling, cohesion, substitution, or extension pressure?
- Are interfaces, repositories, factories, containers, layers, or patterns speculative or ceremonial?
- Can any boundary be removed or merged without losing independence or testability?

### Organization and Composition

- Can maintainers find capabilities and ownership boundaries using project-native conventions?
- Do modules change for cohesive reasons, and can independently evolving areas remain independent?
- Are implementation selection, configuration, wiring, lifecycle, and cleanup visible at system edges?
- After contract changes, are affected consumers and composition points complete?

## Findings Contract

For each finding, report:

- observed evidence and affected path;
- violated core invariant or selected contextual policy;
- concrete cost or risk;
- counter-signals and relevant constraints;
- smallest reversible improvement;
- focused verification and residual risk.

Deduplicate findings by root cause. Prioritize invariant violations and demonstrated change cost over naming or style. Do not flag file size, one-unit-per-file, repository-per-entity, aggregate transaction style, strict typing, framework isolation, immutable containers, or compatibility shims unless the project selected that policy or evidence shows a concrete problem.
