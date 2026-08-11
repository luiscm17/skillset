# Architecture Audit Checklist

Use only for an explicitly requested full audit. Read all references sequentially, gather evidence, and produce one prioritized, deduplicated result. Use one architecture worker unless independent uncertainty justifies bounded delegation.

## Establish Scope

- Record requested outcome, current vocabulary, ecosystem constraints, compatibility obligations, and available verification.
- Map product capabilities, owners, consumers, composition points, public entry points, and shared surfaces.
- Record selected contextual heuristics and opt-in organization policies; do not infer them from names.

## Core Invariants

- Is organization capability-first rather than globally grouped by technical type?
- Does every capability expose a narrow public contract while keeping internals private?
- Do all cross-capability collaborations use explicit contracts and stable entry points?
- Does composition depend on capabilities without reverse dependency?
- Is shared code small, explicitly owned, and free of accidental product policy?
- Does every responsibility and value have one canonical owner and source?
- Are derived values computed or projected rather than synchronized as competing state?

## Contextual Evidence

- Which responsibilities change together, and which change independently?
- Do conceptual zones improve ownership and navigation, or add forwarding ceremony?
- Are contracts consumer-relevant, cohesive, semantically precise, and evolvable?
- Do imports, callbacks, messages, state access, and runtime registration preserve boundaries?
- Does reuse have equivalent meaning, lifecycle, ownership, and observed pressure?
- Are migrations one-directional, compatibility-bounded, and tied to retirement proof?
- Can a flatter or more local design preserve the same invariants?

## Findings Contract

For each finding, report evidence and affected path; violated core invariant or selected policy; concrete cost; counter-signals; smallest reversible improvement; contract, ownership, and dependency effects; focused verification; migration and retirement conditions; residual risk.

Deduplicate by root cause. Prioritize invariant violations and demonstrated change cost over naming, directory shape, abstraction preference, or hypothetical scale.
