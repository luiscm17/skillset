# Diagnostic Signals

Treat these as hypotheses. Confirm observable cost and counter-signals before changing architecture; `../SKILL.md` remains canonical.

| Suspected problem | Evidence to seek | Small response |
| --- | --- | --- |
| Global technical-type organization | One capability change scatters across unrelated global buckets and ownership is hard to find | Move one cohesive capability slice behind a public surface. |
| Internal reach-through | Consumers import private state, helpers, or representation from another capability | Expose the smallest semantic contract and migrate one consumer path. |
| Shell-owned product policy | Composition interprets capability rules or becomes a second authority | Return the decision to its canonical capability owner. |
| Shared dumping ground | Shared code has mixed owners, product decisions, or unrelated consumers | Assign ownership and move capability-specific policy home. |
| Reuse by appearance | Similar-looking interface code has different meaning, lifecycle, or change pressure | Keep variants local or share only proven mechanics. |
| Duplicate authority | The same value or decision is written in multiple places and synchronized | Select one canonical source and derive the rest. |
| Contract leaks internals | Internal representation or technical dependencies become consumer obligations | Publish consumer-relevant semantics and translate at the boundary. |
| Zone ceremony | Empty layers, forwarding modules, or uniform trees add navigation without independence | Flatten the capability until real pressure justifies separation. |
| Capability cycle | Mutual dependencies force coordinated internal knowledge | Revisit ownership, define collaboration contracts, or merge a false boundary. |
| Permanent migration | Old and new paths both write or lack retirement proof | Establish one direction, bounded compatibility, and removal evidence. |

Do not diagnose from directory names, file counts, visual variation, or use of a particular implementation technique alone. Rank invariant violations above contextual improvements and reject speculative abstractions.
